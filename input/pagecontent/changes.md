# Changes

## 2026-10-20

### Data

- The guide now describes the full set of finished vaccine products published
  by the WHO PreQual database. Earlier builds described only a subset, because
  the importer paged the source API with a parameter name the service does not
  recognise and every request returned the first page of results.
- The service caps how many records one response carries, independently of the
  page size requested. Pagination now advances by the number of records
  actually returned, and the import fails rather than publishing an incomplete
  extract.
- Products whose detail has been **de-published** are now included. The source
  database clears a product's fields when it leaves prequalified status,
  retaining its identifier, final status and last publishing date. These
  records are the authoritative account of a past prequalification.
- A de-published product keeps its instance identifier, so consumers observe a
  change of status rather than a record disappearing. Detail published before
  the withdrawal is not retained, because this guide reflects what the source
  database currently publishes.
- Status values now cover the whole prequalification lifecycle. Codes for
  cancelled, rejected and in-assessment states have been added to
  `PreQualDatabaseMetadata`. Consumers needing only current products should
  filter on `status = #Prequalified`.
- Where an applicant or regulator name is empty but its identifier is present,
  the name is taken from another record carrying the same identifier. No name
  is inferred or constructed.

### Logical model

- `FinishedVaccineProducts` now requires only `productId`, `productName` and
  `status`. These are the only elements present on every record in the source
  database.
- Relaxed from `1..1` to `0..1`: `productType`, `dateOfPrequal`,
  `presentation`, `vaccineCommercialName`, `applicantName`, `nraName`,
  `manufacturerReference`, `responsibleNRAReference`.
- `applicantName` and `nraName` were previously documented as always present.
  A full extract disproved that.
- `manufacturerReference` and `responsibleNRAReference` are present only
  alongside the corresponding name, because the referenced `Organization`
  instances are keyed on it.
- **Implementers should treat every element except `productId`, `productName`
  and `status` as optional**, and should not assume an instance carries a
  product or regulator name.

### Known data quality items

Raised with the WHO PreQual team and unresolved at the date of this extract:

- A small number of prequalified products have no responsible NRA recorded.
- A small number have no date of prequalification acceptance.
- Some applications that have not completed prequalification are flagged as
  published snapshots in the source database.
- `ProductIngredients` records carry no identifier, because the source query
  does not select one, so ingredient instances cannot be identified stably
  across extracts.
- No NRA address field is selected by the source query, so
  `PreQualNRA.addressLine1` is never populated.
