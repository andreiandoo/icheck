# ANAF Integration

Status: **READY / MVP CORE**

## Scope

ANAF is the primary provider for company fiscal and financial checks.

Initial capabilities:

```text
anaf.vat
anaf.vat_cash_accounting
anaf.fiscal_status
anaf.efactura
afnf.financials
anaf.tax_arrears_public_list
```

> Note: capability names are internal and may change before implementation. Keep product semantics stable even if ANAF endpoint structure changes.

## MVP products depending on ANAF

- EV-COMP-001 Company lookup baseline
- EV-FISC-001 VAT
- EV-FISC-002 RO e-Factura
- EV-FISC-003 Active/inactive taxpayer
- EV-FISC-004 Financial snapshot
- EV-FISC-005 Financial evolution
- EV-FISC-006 Public tax arrears
- EV-COMP-010 Company Passport

## Authentication

ANAF provides OAuth infrastructure for third-party applications for APIs that require authenticated access. During implementation, document per capability whether it is:

- public service;
- OAuth-protected;
- certificate-dependent;
- batch-based.

Never use one global assumption for all ANAF services.

## Technical requirements

Each capability spec must define:

- official endpoint;
- HTTP method;
- auth mechanism;
- request schema;
- response schema;
- max batch size;
- rate limit;
- timeout;
- freshness/cache policy;
- error mapping;
- fixture examples.

## Company identifier

Canonical Romanian company identifier is normalized CUI.

Normalization rules:

- trim whitespace;
- uppercase;
- remove optional `RO` prefix for canonical storage;
- validate numeric form/check digit where appropriate;
- preserve original customer input only as metadata.

## e-Factura registry

Known integration characteristics to verify at implementation time:

- POST lookup endpoint;
- batch lookup supported;
- service-specific throttling exists;
- response includes company identity/registry state fields.

Do not hard-code documentation assumptions until endpoint contract is captured in tests.

## Financial statements

Normalize annual observations into a stable internal model:

```text
fiscal_year
revenue
net_profit_loss
assets
liabilities
equity
employees
source_report_type
source_published_at nullable
```

Calculated metrics belong to EU-VERIFIC, not provider adapter:

- YoY revenue growth;
- profit margin;
- debt ratios;
- revenue/employee;
- trend findings.

## Public tax arrears

Important semantic rule:

Absence from the public ANAF list does **not** mean the company has no tax debt. Customer-facing finding must explicitly refer to absence/presence in the published list and snapshot date.

## Source snapshot policy

Store:

- normalized request metadata;
- raw response where licensing/privacy allows;
- retrieval timestamp;
- endpoint/capability version;
- checksum;
- parser version.

For downloadable/public lists, preserve source resource URL/version/date and import checksum.

## Caching

To be finalized per capability. General rule:

- identity/annual financials can use longer cache;
- VAT/fiscal state should have shorter freshness;
- public lists inherit publication cadence;
- paid verification must display `verified_at` for the actual snapshot used.

## Errors

Map to platform categories:

- `NOT_FOUND`
- `INVALID_REQUEST`
- `RATE_LIMITED`
- `AUTH_FAILED`
- `PROVIDER_TIMEOUT`
- `PROVIDER_UNAVAILABLE`
- `PROVIDER_RESPONSE_INVALID`

## Tests

Required before MVP:

- valid VAT company;
- non-VAT company;
- active/inactive taxpayer cases;
- e-Factura enrolled/not enrolled;
- company with financial history;
- company with missing year;
- malformed/unknown CUI;
- upstream timeout/rate limit;
- fixture/schema change detection.

## Open questions

See `docs/09-open-questions.md`.
