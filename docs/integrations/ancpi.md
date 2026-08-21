# ANCPI Integration

Status: **CONDITIONAL + WORKFLOW / MVP DOCUMENT CORE**

## Scope

ANCPI/OCPI provides property data, official documents and cadastral/publicity workflows.

EU-VERIFIC must separate three access modes:

1. **commercial deponent/intermediary** — the company can orchestrate and pay requests for customers where permitted;
2. **data/API access** — licensed/approved access to ANCPI datasets/API packages;
3. **authorized cadastral professional/company** — regulated operations and professional eTerra workflows.

These are not interchangeable.

## MVP capabilities

```text
ancpi.land_registry_extract
ancpi.cadastral_plan
ancpi.archive_document
```

Archive document types:

```text
LAND_BOOK_EXTENSO
REGISTRATION_DECISION
PAD
FLOOR_PLAN
TITLE_DEED
```

## Post-MVP capabilities

```text
ancpi.property_by_address
ancpi.property_by_owner
ancpi.update_property_address
ancpi.note_building_authorization
ancpi.register_building
ancpi.property_resolver
```

## Product strategy

MVP must **not** depend on obtaining ANCPI P3/P4 API access. Build document workflows provider-agnostically so human-assisted/manual submission can be replaced later by an approved M2M adapter.

## API packages

ANCPI tariff documentation confirms API/data packages exist, including datasets around land, constructions, inscriptions/persons and dedicated address/parcel/building APIs. Exact endpoint/auth/schema documentation is not public in the research baseline and must be obtained/confirmed through ANCPI.

Do not invent endpoint contracts.

## Document ordering architecture

```text
DocumentOrder
→ product-specific requirements
→ mandate/legal basis where required
→ payment
→ submission adapter
→ ANCPI/OCPI processing
→ provider reference
→ result document
→ document vault
→ delivery
```

Submission adapter may initially be:

- operations/manual provider;
- authorized partner;
- official web workflow;
- future official API/M2M.

## Archive document unification

Commercially, PAD/releveu/title deed/registration decision are separate SKUs. Technically they should share one `ArchiveDocumentRequest` workflow with `document_type`.

## Property identifiers

Baseline fields:

- county/UAT;
- land book number;
- cadastral number;
- address;
- provider-specific identifiers.

Property resolution must remain explicit because customer inputs can be incomplete or inconsistent.

## Restricted owner lookup

Do not expose anonymous reverse lookup by person/CNP.

Any owner/person-based capability must require:

- access approval;
- authenticated requester;
- purpose/legal basis;
- audit;
- mandate/interest evidence where applicable.

## Source/document evidence

For official documents store:

- issuing provider;
- provider reference/order number;
- issue date;
- received timestamp;
- checksum;
- original PDF in private object storage;
- relationship to customer order.

## Provider downtime

ANCPI/eTerra availability must not block checkout. Orders can remain in `PROVIDER_DELAYED` / processing queue and resume when source is available.

## Open questions

Critical items are tracked in `docs/09-open-questions.md`, especially:

- commercial-company eligibility for API access;
- P3/P4 conditions;
- authentication/rate limits;
- M2M document ordering;
- sandbox;
- dedicated dataset semantics.
