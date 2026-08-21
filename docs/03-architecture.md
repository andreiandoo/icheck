# Architecture

## Style

EU-VERIFIC will be built as a **modular monolith** in Laravel.

The purpose of this choice is to keep deployment and local development simple while preserving strict domain boundaries. External institution integrations are adapters, not business modules.

## Runtime stack

- PHP 8.4
- Laravel
- Livewire
- Blade
- PostgreSQL
- Redis
- S3-compatible object storage

## High-level architecture

```text
Web / Livewire
     ↓
Application Services
     ↓
Domain Modules
     ↓
Ports / Contracts
     ↓
Provider Adapters
     ↓
ANAF / Justice / ANCPI / ONRC / ANRE / ARR / data.gov.ro
```

## Core rules

1. Domain logic must not depend directly on HTTP clients or provider SDKs.
2. External calls execute asynchronously unless a specific check is proven safe and fast enough for synchronous use.
3. Raw source evidence is stored before interpretation.
4. Findings are deterministic and versioned by rule/parser version.
5. Reports aggregate findings; they do not directly parse provider payloads.
6. Official-document workflows are separate from instant checks.
7. Provider downtime must degrade gracefully.
8. Sensitive access is auditable.

## Proposed folder structure

```text
app/
├── Domain/
│   ├── Companies/
│   ├── Properties/
│   ├── Licensing/
│   ├── Litigation/
│   ├── Funding/
│   ├── Checks/
│   ├── Reports/
│   ├── Documents/
│   ├── Monitoring/
│   ├── Orders/
│   └── Billing/
├── Integrations/
│   ├── Anaf/
│   ├── Justice/
│   ├── Ancpi/
│   ├── Onrc/
│   ├── Anre/
│   ├── Arr/
│   └── DataGov/
└── Support/
    ├── Audit/
    ├── Providers/
    ├── RateLimiting/
    ├── Files/
    └── Observability/
```

This is a target structure, not a requirement to create every folder before code exists.

## Provider capabilities

Providers expose explicit capabilities rather than generic remote-method calls.

Examples:

```text
anaf.vat
anaf.efactura
anaf.fiscal_status
anaf.financials
justice.litigation
anre.company_license
arr.carrier_license
ancpi.land_registry_extract
ancpi.cadastral_plan
ancpi.archive_document
```

Conceptual contract:

```php
interface CheckProvider
{
    public function supports(string $capability): bool;

    public function check(
        Subject $subject,
        CheckRequest $request
    ): CheckResult;
}
```

Provider adapters are responsible for transport/protocol details. Domain/application services are responsible for product semantics.

## Asynchronous orchestration

Company Passport example:

```text
CreateReport
    ↓
Resolve required capabilities
    ↓
Create check runs
    ↓
Dispatch provider jobs in parallel
 ┌───────┬───────────┬──────────┐
 ANAF    Justice     ANRE      ...
 └───────┴───────────┴──────────┘
    ↓
Source snapshots
    ↓
Normalize
    ↓
Generate findings
    ↓
Compile report
    ↓
READY / PARTIAL / FAILED
```

## Queue strategy

Recommended named queues:

```text
critical
checks
reports
documents
sync
monitoring
notifications
```

Provider-specific concurrency/rate limits should be implemented via Redis locks/throttles, not by assuming queue worker counts are sufficient.

## Idempotency

Every external operation needs a stable idempotency/fingerprint strategy.

Examples:

- check request fingerprint: provider + capability + normalized subject + effective date/context;
- document-order submission key: order + workflow step;
- provider sync: provider + dataset/version/date.

Retries must never create duplicate paid orders or duplicate official submissions.

## Caching and freshness

Caching policy is capability-specific.

Examples:

- company identity: relatively long cache;
- TVA/fiscal status: shorter cache;
- litigation: short/medium cache;
- license registry dataset: based on provider refresh frequency;
- official document: never replaced by cached informational data.

Each capability must define:

- `freshness_ttl`;
- whether stale-while-revalidate is allowed;
- whether customer can force refresh;
- whether a cached result can be sold as a new verification.

## Source snapshots

Raw evidence should be immutable after creation.

Suggested fields:

```text
id
provider_id
capability
subject_type
subject_id
request_fingerprint
request_metadata jsonb
raw_payload jsonb nullable
artifact_path nullable
retrieved_at
checksum
parser_version
created_at
```

Large PDFs/binaries belong in object storage, not PostgreSQL.

## Findings engine

Findings are produced by rules applied to normalized source data.

Example:

```text
VAT_ACTIVE -> POSITIVE
FISCAL_INACTIVE -> CRITICAL
TAX_ARREARS_PUBLIC_LIST -> WARNING or CRITICAL depending on product semantics
INSOLVENCY_CASE -> CRITICAL
REVENUE_DECLINE_20 -> WARNING
LICENSE_EXPIRING_30_DAYS -> WARNING
```

Rules should have stable identifiers and versions.

## Reports

Report content should be section-based and composed from findings/snapshots.

```text
CompanyPassport
├── Identity
├── Fiscal
├── Financial
├── Litigation
├── Licensing
└── Summary
```

The HTML report is the canonical presentation. PDF is a generated representation of the same report data.

## Documents

Document ordering has its own aggregate/workflow because it involves:

- customer inputs;
- required supporting files;
- mandates/legal basis;
- provider fees;
- payment;
- submission;
- external processing;
- completion requests;
- final document delivery.

## Provider health

Every provider should expose operational state:

```text
UP
DEGRADED
DOWN
MANUAL_ONLY
UNKNOWN
```

Health checks must not create excessive traffic to state systems. Prefer lightweight capability probes or infer health from recent job metrics.

## Observability

Minimum metrics:

- external request duration;
- external request failure rate;
- rate-limit events;
- check completion time;
- document-order age by state;
- queue backlog;
- failed jobs;
- report compilation duration.

Logs must avoid dumping personal/sensitive raw payloads by default.

## API-first internally

Even though the frontend is Blade + Livewire, use-cases should be represented by application services/commands that can later be exposed through a B2B API without rewriting core logic.

The UI should not contain provider business logic.

## Security boundaries

- separate customer roles from operational/admin roles;
- provider credentials encrypted and never logged;
- signed/private document URLs;
- authorization policies for documents and reports;
- strict auditing for restricted lookups;
- rate limit customer-facing search endpoints.

## Deployment assumption

Initial deployment can be a standard Laravel production topology:

```text
Load balancer / web
Laravel application
PostgreSQL
Redis
Queue workers
Scheduler
Object storage
```

No Kubernetes or microservice infrastructure is required by the MVP.
