# Development Roadmap

## Guiding rule

Do not block the MVP on the hardest institutional integrations. Build the product core and use provider abstractions/workflows so deeper integrations can replace manual steps later.

## Sprint 0 — Platform Core

Goals:

- Laravel bootstrap;
- PostgreSQL;
- Redis;
- authentication;
- subjects;
- providers/capabilities;
- checks/check runs;
- source snapshots;
- findings;
- audit;
- queue topology;
- object storage abstraction;
- baseline observability;
- tests and CI.

Deliverables:

- project boots locally and in CI;
- database migrations for core entities;
- provider contract interfaces;
- fake provider for end-to-end tests;
- admin/debug view for check runs;
- architecture ADRs committed.

## Sprint 1 — ANAF

Goals:

- company identity lookup baseline;
- fiscal status;
- VAT;
- e-Factura;
- financials;
- public tax arrears ingestion/check;
- provider throttling;
- fixtures/contract tests;
- source snapshots.

Deliverable:

A company page that can resolve and show official-source fiscal/financial checks with source timestamp.

## Sprint 2 — Justice + Company Passport

Goals:

- Justice provider;
- company-name matching strategy;
- litigation normalization;
- finding rules;
- report engine;
- Company Passport Livewire UI;
- HTML report;
- PDF generation.

Deliverable:

End-to-end Company Passport without checkout dependency.

## Sprint 3 — Commerce

Goals:

- product catalog persistence/config;
- orders/order items;
- checkout;
- payment provider;
- invoices integration strategy;
- customer account;
- report/document history;
- notifications.

Deliverable:

Paid Company Passport end-to-end.

## Sprint 4 — ANCPI Document Workflows

Goals:

- document-order aggregate;
- product-specific requirement definitions;
- property baseline identity;
- uploads;
- mandates/consents;
- operational admin queue;
- submission tracking;
- provider references;
- document vault;
- delivery notifications.

Initial SKUs:

- Extras CF;
- cadastral plan;
- CF extenso;
- registration decision;
- PAD;
- floor plan;
- title deed copy.

Deliverable:

Human-assisted ANCPI workflow that can later receive a machine-to-machine adapter.

## Sprint 5 — ANRE + Monitoring Foundation

Goals:

- ANRE registry sync;
- normalized licenses;
- contractor check;
- monitoring subscription model;
- change detection;
- monitoring events;
- notifications.

Deliverable:

Contractor verification and first recurring monitoring use case.

## Post-MVP — Priority candidates

1. monitoring productization and pricing;
2. Supplier/Customer Risk Passport;
3. ARR integration after access clarification;
4. ONRC M2M/document integration;
5. ANCPI P3/P4 data capabilities if approved;
6. Property Passport;
7. funding/MIPE checks;
8. B2B API;
9. bulk CSV verification;
10. organization/team features.

## B2B API phase

Expose stable application capabilities, not raw provider APIs.

Candidate endpoints:

```text
GET /api/v1/companies/{cui}
GET /api/v1/companies/{cui}/fiscal
GET /api/v1/companies/{cui}/financials
GET /api/v1/companies/{cui}/litigation
POST /api/v1/company-reports
GET /api/v1/contractors/{cui}
POST /api/v1/document-orders
POST /api/v1/monitoring-subscriptions
```

## Before each sprint

Required:

- write or update detailed technical spec;
- settle unresolved legal/provider questions that affect scope;
- define acceptance criteria;
- define test fixtures;
- identify provider fallback.

## Definition of Ready for implementation

A feature is ready when:

- product behavior is clear;
- provider capability is known;
- input/output is specified;
- lifecycle/statuses are defined;
- compliance constraints are documented;
- failure behavior is defined;
- acceptance tests can be written.

## Definition of Done

A feature is done when:

- tests pass;
- audit behavior exists where required;
- external calls have timeout/retry policies;
- raw evidence/source reference is preserved;
- errors are observable;
- user-facing wording matches source semantics;
- operational fallback exists for paid workflows.
