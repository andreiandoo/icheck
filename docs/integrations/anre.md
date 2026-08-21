# ANRE Integration

Status: **DATA_FEED / MVP CANDIDATE**

## Scope

ANRE registries can support contractor/license verification for energy and gas verticals.

Initial internal capabilities:

```text
anre.company_gas_license
anre.company_electricity_license
```

Post-MVP:

```text
anre.person_electrician_license
```

## Strategy

Prefer periodic synchronization of official registry/export data over live scraping per customer request.

Architecture:

```text
Official registry/export
→ scheduled sync
→ immutable import snapshot
→ normalized licenses
→ company matching by CUI/name
→ customer check
```

## Normalized license model

```text
provider_license_id
company_id nullable
holder_name
holder_identifier nullable
license_type
license_number nullable
issued_at nullable
valid_from nullable
valid_until nullable
status
source_snapshot_id
```

## Freshness

Each result must display source update/import timestamp. Monitoring should compare semantic license state, not raw row differences.

## Risks

- export format changes;
- missing/unstable identifiers;
- company name matching when CUI absent;
- unclear commercial reuse terms;
- registry update frequency.

## Tests

- valid active license;
- expired license;
- multiple licenses per company;
- company name variants;
- duplicate rows;
- changed export schema;
- company not found.

## Open questions

- reuse/license conditions;
- stable API/feed availability;
- unique identifiers;
- update frequency;
- historical revocation/suspension data.
