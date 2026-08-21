# data.gov.ro / MIPE Integration

Status: **READY / DATA_FEED / POST-MVP**

## Scope

Open datasets published through data.gov.ro can support EU funding and other future intelligence products.

Initial candidate capabilities:

```text
datagov.mipe_projects
datagov.mipe_beneficiaries
```

## Strategy

Use CKAN/resource metadata to discover and version datasets, then import resources into a staging pipeline.

```text
resource metadata
→ download/import job
→ checksum/version detection
→ staging
→ normalization
→ entity resolution by CUI/name
→ canonical funding records
```

## Do not query large CSV resources at customer request time

Customer checks should query normalized local PostgreSQL tables built from official resource snapshots.

## Dataset provenance

For every import store:

```text
provider
dataset_id
resource_id
source_url
license
published/modified timestamp
retrieved_at
checksum
importer_version
row_count
status
```

## Normalized funding model candidate

```text
company_id nullable
beneficiary_name
beneficiary_identifier nullable
project_code
project_name
program
contract_value nullable
grant_value nullable
start_date nullable
end_date nullable
status nullable
source_import_id
```

## Data quality

Open data can be stale, incomplete or inconsistent. Reports must display dataset/source update date and avoid implying real-time status.

Entity resolution should classify matches as:

- exact CUI;
- exact normalized name;
- probable name match;
- ambiguous/unmatched.

Only sufficiently reliable matches should appear as confirmed company findings.

## Open questions

- which MIPE datasets remain actively maintained;
- exact reuse licenses per resource;
- CUI coverage;
- update cadence;
- resource replacement/versioning behavior;
- minimum freshness acceptable for a commercial Funding Passport.
