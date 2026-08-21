# MVP Scope

## Obiectiv

MVP-ul validează două motoare distincte ale EU-VERIFIC:

1. verificări și rapoarte aproape instant;
2. comenzi și workflows pentru documente oficiale.

Nu încercăm să lansăm simultan toate instituțiile și toate verticalele.

## Primary user journeys

### Journey A — Verifică o companie

1. utilizatorul introduce CUI-ul sau denumirea;
2. platforma identifică societatea;
3. afișează basic identity gratuit;
4. utilizatorul cumpără verificări sau Company Passport;
5. providers sunt interogați asincron;
6. source snapshots sunt păstrate;
7. findings sunt generate;
8. raportul este afișat și disponibil PDF.

### Journey B — Obține un document imobiliar

1. utilizatorul selectează documentul;
2. introduce identificatorii imobilului;
3. primește checklist-ul de date/documente;
4. completează mandatul/cerințele aplicabile;
5. plătește;
6. request-ul intră în workflow;
7. provider/operatorul procesează cererea;
8. documentul oficial este încărcat;
9. utilizatorul este notificat și îl poate accesa din cont.

## In scope — Company

- `EV-COMP-001` Company lookup;
- `EV-FISC-001` TVA;
- `EV-FISC-002` RO e-Factura;
- `EV-FISC-003` activ/inactiv fiscal;
- `EV-FISC-004` situații financiare;
- `EV-FISC-005` evoluție financiară;
- `EV-FISC-006` restanțe fiscale publicate;
- `EV-LIT-001` litigation lookup;
- `EV-LIT-002` litigation report;
- `EV-COMP-010` Company Passport.

## In scope — Property

- `EV-PROP-001` Extras CF;
- `EV-PROP-002` Plan cadastral / ortofoto;
- `EV-PROP-010` CF extenso;
- `EV-PROP-011` Încheiere intabulare;
- `EV-PROP-012` PAD;
- `EV-PROP-013` Releveu;
- `EV-PROP-014` Act proprietate.

MVP-ul nu depinde de API-urile avansate ANCPI P3/P4.

## In scope — Licensing

- `EV-LIC-001` firmă autorizată ANRE gaze;
- `EV-LIC-002` firmă autorizată ANRE electricitate.

## In scope — Platform

- registration/login;
- company/property lookup UI;
- catalog servicii;
- product detail pages;
- shopping/order flow;
- payments abstraction;
- customer account;
- order history;
- report viewer;
- generated PDF reports;
- document vault;
- admin operational queue;
- provider health;
- audit trail;
- notifications baseline;
- failed-job retry tooling.

## Explicitly out of scope

- SEAP;
- public B2B API;
- bulk CSV verification;
- full monitoring suite;
- EU-VERIFIC Score numeric;
- beneficial owners;
- anonymous person lookup;
- ARR until access is clarified;
- ANCPI P3/P4 dependency;
- advanced Property Passport;
- AI-generated risk decisions;
- native mobile apps;
- microservices.

## MVP success criteria

### Technical

- provider failure does not lose customer orders;
- complete audit trail for checks/documents;
- source evidence is reproducible;
- report generation is deterministic;
- provider contracts have automated tests;
- no external provider blocks the web request lifecycle;
- operational users can recover a failed request from admin UI.

### Product

- user can obtain a Company Passport end-to-end;
- user can order at least the core ANCPI document set end-to-end;
- every report displays source + timestamp;
- every order exposes clear status;
- user can access previous reports/documents from account.

### Business

Track from day one:

- lookup → purchase conversion;
- average order value;
- gross margin per SKU;
- provider cost per order;
- support rate;
- completion rate;
- repeat purchase rate.

## Launch sequencing

### Internal Alpha

- Company checks;
- report engine;
- no payments required;
- provider/debug UI.

### Private Beta

- payments;
- Company Passport;
- ANCPI document workflows;
- customer accounts.

### Public MVP

- production payments;
- full customer status/notifications;
- operational admin;
- ANRE checks if sync is stable.

## Non-functional baseline

- timezone: Europe/Bucharest;
- application locale: Romanian first;
- UTF-8 throughout;
- immutable timestamps in UTC, displayed in local timezone;
- database: PostgreSQL;
- queue/cache: Redis;
- files: S3-compatible storage;
- external HTTP calls must define connect/read timeout;
- secrets must never be stored in source control.
