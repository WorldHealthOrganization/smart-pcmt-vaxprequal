# Changes

This page records changes to the data and to the logical models in this
implementation guide.

## 2026-10-20

### Complete prequalification records

Earlier builds of this guide described only a small subset of the products
published by the WHO PreQual database. They now describe the full set.

The shortfall was not an editorial decision. The importer paged through the
source API using a parameter name the service does not recognise, so every
request returned the first page of results, and the repeated pages collapsed
to a single page's worth of products once duplicates were removed. The
service also limits how many records one response can carry, independently
of the page size requested. Pagination now advances by the number of records
actually returned, and the import fails rather than publishing an incomplete
extract.

### Records with no published detail

The source database **de-publishes** a product's detail when that product
leaves prequalified status: the record is retained with its identifier, its
final status and the date it was last published, and the remaining fields
are cleared.

These records are the authoritative account of a past prequalification, so
they are now published. They appear as `FinishedVaccineProducts` instances
carrying `productId`, `productName`, `status` and `lastPublishingDate` only:

```
Instance: FinishedProduct<identifier>
InstanceOf: FinishedVaccineProducts
* status = #WithdrawnWasPrequalified
* productId.value = "<identifier>"
* productName = "<record name>"
* lastPublishingDate = <date>
```

A product that is withdrawn after appearing in this guide keeps its instance
identifier, so consumers tracking a product observe a change of status rather
than a record disappearing. Detail published before the withdrawal is not
retained, because this guide reflects what the source database currently
publishes.

### Cardinality of FinishedVaccineProducts relaxed

Only `productId`, `productName` and `status` are present on every record in
the source database, so only those remain mandatory. The following elements
changed from `1..1` to `0..1`:

| Element | Why it can be absent |
|---|---|
| `productType` | cleared on de-published records |
| `dateOfPrequal` | cleared on de-published records; also absent on a small number of prequalified products |
| `presentation` | cleared on de-published records |
| `vaccineCommercialName` | cleared on de-published records |
| `applicantName` | cleared on de-published records; occasionally not populated upstream |
| `nraName` | cleared on de-published records; occasionally not populated upstream |
| `manufacturerReference` | present only where an applicant name is available |
| `responsibleNRAReference` | present only where an NRA name is available |

`applicantName` and `nraName` were previously documented as always present.
A full extract disproved that. The two organization references are present
only alongside the corresponding name, because the referenced `Organization`
instances are keyed on it.

**Implementers should treat every element except `productId`, `productName`
and `status` as optional**, and should not assume a `FinishedVaccineProducts`
instance carries a product or regulator name.

### Product status

Status values now cover the whole prequalification lifecycle rather than only
prequalified and withdrawn. Codes for cancelled, rejected and in-assessment
states have been added to `PreQualDatabaseMetadata` as the source data
presents them.

Records that have not completed prequalification are included, because this
guide reflects the source database as published. Consumers that require only
currently prequalified products should filter on `status = #Prequalified`.

### Organization names recovered from identifiers

Applicant and regulator names in the source API are denormalized copies that
are occasionally empty while the underlying relationship identifier is
intact. Where a name is missing and the identifier is present, the name is
now taken from another record carrying the same identifier. The value is
always one the source API supplied for that identifier; no name is inferred
or constructed.

### Known data quality items

Raised with the WHO PreQual team and unresolved at the date of this extract:

- a small number of prequalified products have no responsible NRA recorded
- a small number of prequalified products have no date of prequalification
  acceptance
- some applications that have not completed prequalification are flagged as
  published snapshots in the source database
- `ProductIngredients` records carry no identifier, because the source query
  does not select one, so ingredient instances cannot be identified stably
  across extracts
- no NRA address field is selected by the source query, so
  `PreQualNRA.addressLine1` is never populated
