# Changes

This page records changes to the data and to the logical models in this
implementation guide.

## 2026-10-20

### All prequalification records are now included

Earlier builds of this guide described **35** finished vaccine products. They
now describe the full set published by the WHO PreQual database — **271**
records as of this extract.

The shortfall was not an editorial decision. The importer paged through the
source API using a parameter name the service does not recognise, so every
request returned the first page of results. Repeated pages collapsed to the
same 35 products once duplicates were removed. The service also limits how
many records a single response can carry, independently of the page size
requested, so pagination now advances by the number of records actually
returned and the import fails rather than publishing a short extract.

### Records with no published detail are now represented

The source database **de-publishes** a product's detail when that product
leaves prequalified status: the record is retained with its identifier, its
final status and the date it was last published, and the remaining fields are
cleared. 22 of the 271 records are in this state.

These records are the authoritative account of a past prequalification, so
they are now published. They appear as `FinishedVaccineProducts` instances
carrying `productId`, `productName`, `status` and `lastPublishingDate` only,
for example:

```
Instance: FinishedProducta3K3X000006Mii4UAC
InstanceOf: FinishedVaccineProducts
* status = #WithdrawnWasPrequalified
* productId.value = "a3K3X000006Mii4UAC"
* productName = "FVP-P-292"
* lastPublishingDate = 2025-07-08
```

A product that is withdrawn after being published in this guide keeps its
instance identifier, so consumers tracking a product observe a change of
status rather than a record disappearing. The detail published before the
withdrawal is not retained, because this guide reflects what the source
database publishes.

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
A full extract disproved that, and the two organization references are
present only alongside the corresponding name, because the referenced
`Organization` instances are keyed on it.

**Implementers should treat every element except `productId`, `productName`
and `status` as optional**, and should not assume a `FinishedVaccineProducts`
instance carries a product or regulator name.

### Additional status codes

`PreQualDatabaseMetadata` previously carried two product statuses. The full
extract contains nine, and the additional codes have been added:
`Acceptable`, `CancelledAssessment`, `DecisionPhase`, `RejectedScreening`,
`UnderAssessment`, `UnderScreening` and `WithdrawnWasAccepted`.

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

Reported to the WHO PreQual team and unresolved at the date of this extract:

- `FVP-P-98` is prequalified with no responsible NRA identifier and no NRA name
- `FVP-P-216` and `FVP-P-330` are prequalified with no date of prequalification acceptance
- `FVP-P-app8` has a record name that does not follow the `FVP-P-<number>`
  convention, and no presentation or commercial name, while marked prequalified
- Six applications that have not completed prequalification, including one
  rejected at screening, are flagged as published snapshots in the source
  database
- `ProductIngredients` records carry no identifier, because the source query
  does not select one, so ingredient instances cannot be identified stably
  across extracts
- No NRA address field is selected by the source query, so
  `PreQualNRA.addressLine1` is never populated
