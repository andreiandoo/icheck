# Domain Model

## Purpose

The domain model separates customer-facing product concepts from provider-specific payloads. A company is a company whether its fiscal status comes from ANAF or another future source.

## Core aggregates and entities

### Subject

Abstract identity of the entity being checked.

Initial subject types:

- `Company`
- `Property`
- `LicensedOperator`

A generic `subjects` record can provide a common reference for checks/reports while type-specific data lives in dedicated tables.

### Company

Suggested fields:

```text
id
subject_id
country_code
cui
registration_number nullable
legal_name
normalized_name
address jsonb nullable
status_metadata jsonb nullable
created_at
updated_at
```

Rules:

- Romanian CUI must be normalized before lookup;
- `RO` VAT prefix is presentation/input metadata, not part of canonical numeric CUI;
- company identity from providers must be reconciled, not blindly overwritten.

### Property

Suggested baseline:

```text
id
subject_id
country_code
county_code nullable
uat nullable
land_book_number nullable
cadastral_number nullable
address_text nullable
normalized_address jsonb nullable
provider_identifiers jsonb nullable
created_at
updated_at
```

Property resolution is expected to evolve once ANCPI access is confirmed.

### LicensedOperator

May reference a Company and one or more registry identities/licenses.

### Provider

Represents an external official source/institution.

Fields:

```text
id
code
name
status
configuration_metadata jsonb
```

Examples: `ANAF`, `JUSTICE`, `ANCPI`, `ONRC`, `ANRE`, `ARR`, `DATAGOV`.

### ProviderCapability

Defines an integration capability.

```text
provider_id
code
status
freshness_ttl
requires_auth
requires_legal_basis
supports_sync
supports_async
metadata jsonb
```

### Check

Customer/application-level request to verify a capability or bundle.

```text
id
subject_id
requested_by nullable
product_code nullable
status
requested_at
completed_at nullable
```

### CheckRun

One execution of one provider capability.

```text
id
check_id
provider_id
capability
status
attempt
started_at
completed_at
error_code nullable
error_metadata jsonb nullable
```

A check can have several check runs if a report uses multiple providers.

### SourceSnapshot

Immutable evidence from a source.

```text
id
check_run_id
provider_id
capability
request_fingerprint
request_metadata jsonb
raw_payload jsonb nullable
artifact_path nullable
content_type nullable
checksum
parser_version
retrieved_at
```

Snapshots should not be mutated after processing. If parsing rules change, create new normalized output/findings that reference the original evidence.

### Finding

Deterministic conclusion derived from evidence.

```text
id
check_id
source_snapshot_id nullable
code
severity
status/value jsonb
rule_version
message_key
metadata jsonb
```

Severity enum candidate:

```text
POSITIVE
INFO
WARNING
CRITICAL
```

### Report

Customer-facing aggregate.

```text
id
subject_id
product_code
status
version
requested_by
compiled_at nullable
expires_at nullable
metadata jsonb
```

### ReportSection

```text
id
report_id
code
position
status
payload jsonb
```

A section should reference findings/snapshots logically rather than duplicating raw evidence.

### DocumentOrder

Aggregate for official document acquisition.

```text
id
subject_id nullable
customer_id
product_code
status
provider_code
provider_reference nullable
official_cost nullable
selling_price
currency
submitted_at nullable
completed_at nullable
```

### DocumentOrderRequirement

Dynamic checklist item:

```text
id
document_order_id
code
kind
required
status
value jsonb nullable
```

Kinds may include:

- field/input;
- uploaded file;
- identity proof;
- mandate;
- consent;
- legal-basis declaration.

### Document

Metadata for files stored in object storage.

```text
id
owner_type
owner_id
kind
storage_disk
storage_path
original_filename
mime_type
size
checksum
issued_by nullable
issued_at nullable
expires_at nullable
```

### Mandate / Consent / LegalBasis

These should be explicit records, not booleans hidden inside an order.

They must preserve:

- text/version accepted;
- actor;
- subject;
- purpose;
- timestamp;
- evidence/reference.

### MonitoringSubscription

```text
id
customer_id
subject_id
status
frequency
capabilities jsonb
last_run_at nullable
next_run_at nullable
```

### MonitoringRun

Represents a periodic re-check execution.

### MonitoringEvent

Represents a material change between snapshots.

```text
id
subscription_id
subject_id
event_code
severity
previous_snapshot_id nullable
current_snapshot_id
occurred_at
notified_at nullable
```

### Order / OrderItem

Commercial transaction should remain separate from operational document/check state.

An order item references a product code and may create a `Check`, `Report`, `DocumentOrder` or subscription after payment.

### Payment / Invoice

Provider-neutral billing records. Payment gateway integration belongs in Billing/Payments adapters.

### AccessAuditLog

Append-only record for sensitive/relevant actions.

```text
id
actor_type
actor_id nullable
action
subject_type nullable
subject_id nullable
provider_id nullable
capability nullable
purpose nullable
legal_basis_reference nullable
request_fingerprint nullable
resource_type nullable
resource_id nullable
ip_hash nullable
created_at
```

## Important invariants

1. A customer cannot access a report/document they do not own or have explicit organization access to.
2. A `READY` report must have all mandatory sections resolved or explicitly marked unavailable/partial.
3. A provider response used in a paid result must have evidence/snapshot metadata.
4. Retrying a `DocumentOrder` submission must not create a duplicate institutional request.
5. Restricted capability runs require a recorded purpose/legal basis before dispatch.
6. Official documents are immutable after receipt; replacements create new document versions.

## Enums to define early

- `ProductType`
- `IntegrationStatus`
- `CheckStatus`
- `CheckRunStatus`
- `ReportStatus`
- `FindingSeverity`
- `DocumentOrderStatus`
- `ProviderHealthStatus`
- `MonitoringStatus`
- `MonitoringEventSeverity`

Use PHP backed enums for stable lifecycle/status values and database constraints where practical.

## PostgreSQL notes

Use `jsonb` only for provider-specific or genuinely schemaless metadata. Core searchable/business fields should remain typed columns.

Recommended:

- unique/partial indexes on canonical identifiers;
- `jsonb` GIN only where query patterns justify it;
- UUID/ULID choice documented in ADR before migrations;
- timestamps with timezone semantics handled consistently by application;
- avoid storing binary documents directly in PostgreSQL.
