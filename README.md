# EU-VERIFIC / iCheck

> **Verifică. Apoi decide.**

EU-VERIFIC este o platformă românească de verificare, documentare și obținere de documente oficiale, construită peste surse publice și servicii digitale ale instituțiilor statului. Produsul transformă date dispersate, registre, API-uri, web services și proceduri administrative în verificări clare, rapoarte ușor de înțeles, documente oficiale și monitorizare continuă.

Repository-ul tehnic folosește numele de lucru **iCheck**. Brandul și domeniul produsului sunt **EU-VERIFIC / eu-verific.ro**.

---

## 1. Viziunea produsului

EU-VERIFIC nu este un agregator generic de date publice și nu este un motor de căutare al registrelor statului. Produsul trebuie să răspundă unor întrebări concrete ale utilizatorilor:

- Pot să lucrez cu firma aceasta?
- Pot să îi dau marfă cu plata la termen?
- Pot să plătesc un avans acestui furnizor?
- Este transportatorul autorizat și activ?
- Este contractorul autorizat?
- Ce risc comercial văd în datele oficiale disponibile?
- Ce documente există pentru imobilul acesta?
- Pot obține rapid un extras CF, PAD, releveu sau alt document oficial?

Promisiunea de produs este simplă:

> **EU-VERIFIC verifică informația în sursa oficială, o structurează și o transformă într-un rezultat pe care poți lua o decizie.**

Pentru documente:

> **EU-VERIFIC gestionează pentru utilizator obținerea documentului oficial.**

---

## 2. Principii de produs

### 2.1 Source-first

Orice informație afișată trebuie să poată fi legată de:

- instituția sursă;
- momentul interogării;
- request-ul sau criteriul folosit;
- payload-ul sau documentul primit;
- versiunea parserului/normalizatorului;
- regula prin care informația brută a devenit un finding.

UI-ul trebuie să poată afișa permanent:

- **Sursa**;
- **Verificat la**;
- **Status**;
- eventual link către sursa oficială.

### 2.2 Explainable, not black-box

EU-VERIFIC poate genera concluzii, findings și ulterior scoruri, dar acestea trebuie să fie explicabile.

MVP-ul va utiliza în principal findings determinate de reguli:

- `POSITIVE`
- `INFO`
- `WARNING`
- `CRITICAL`

Exemple:

- `FISCAL_ACTIVE`
- `VAT_ACTIVE`
- `EFATURA_ACTIVE`
- `TAX_ARREARS_PUBLIC_LIST`
- `REVENUE_DECLINE_20`
- `INSOLVENCY_CASE`
- `LICENSE_EXPIRING`

### 2.3 Official-data semantics

Trebuie evitate concluziile mai puternice decât permit datele oficiale.

Exemplu corect:

> Firma nu apare în lista publică ANAF a obligațiilor fiscale restante la data X.

Exemplu incorect:

> Firma nu are datorii la ANAF.

### 2.4 Async-first

Nicio integrare externă critică nu trebuie presupusă ca fiind disponibilă permanent. Requests către ANAF, ANCPI, Justice, ANRE etc. trebuie orchestrate prin jobs/queues, cu retries, timeouts, circuit breakers și posibilitate de reluare manuală.

### 2.5 No scraping as critical foundation

Nu construim funcționalități business-critical pe scraping neautorizat. Dacă o instituție nu oferă API/feed stabil, integrarea rămâne `CONDITIONAL` sau `WORKFLOW` până la clarificarea accesului.

---

## 3. Tipuri de produse

Catalogul EU-VERIFIC folosește cinci tipuri principale:

| Tip | Descriere | Exemplu |
|---|---|---|
| `INSTANT_CHECK` | verificare automată dintr-o sursă | TVA, e-Factura, litigii |
| `AGGREGATED_REPORT` | raport compus din mai multe surse | Company Passport |
| `OFFICIAL_DOCUMENT` | document emis de o instituție | Extras CF |
| `PROCEDURE` | operațiune administrativă cu workflow | actualizare adresă CF |
| `MONITORING` | reverificare periodică și change detection | monitorizare companie |

---

## 4. Verticale de produs

### 4.1 Companii

Produse inițiale:

- Verificare firmă;
- verificare fiscală;
- TVA;
- TVA la încasare;
- RO e-Factura;
- contribuabil activ/inactiv;
- situații financiare;
- evoluție financiară;
- obligații fiscale restante publicate;
- litigii;
- Company Passport;
- Supplier / Customer Risk Passport.

Surse principale:

- ANAF;
- Ministerul Justiției;
- ONRC, acolo unde accesul comercial permite.

### 4.2 Imobile

Produse:

- Extras de carte funciară;
- Extras plan cadastral pe ortofotoplan;
- Identificare imobil după adresă;
- Identificare imobil după proprietar, numai în cadrul legal permis;
- Actualizare adresă în CF;
- Notare autorizație de construire;
- Înscriere construcție;
- Plan încadrare Ilfov;
- Planuri 1:500 / 1:2000 București;
- Copie CF extenso;
- Copie încheiere intabulare;
- Copie PAD;
- Copie releveu;
- Copie act proprietate;
- Property Passport.

Surse principale:

- ANCPI;
- OCPI;
- parteneri autorizați ANCPI pentru operațiunile reglementate.

### 4.3 Transportatori

Produse planificate:

- Verificare transportator;
- Transportator Passport;
- ulterior monitoring licență.

Sursa principală: ARR.

Această verticală rămâne `CONDITIONAL` până la confirmarea unui canal automatizabil și permis pentru reutilizare comercială.

### 4.4 Contractori / licențe

Produse:

- verificare firmă autorizată ANRE gaze;
- verificare firmă autorizată ANRE electricitate;
- Contractor Passport;
- ulterior alte registre profesionale.

### 4.5 Documente oficiale

Produse:

- documente ANCPI;
- certificat constatator ONRC;
- furnizare informații ONRC;
- istoric societate;
- alte documente oficiale compatibile cu modelul de intermediere.

### 4.6 Finanțări europene

Produse:

- verificare proiecte/finanțări asociate unei companii;
- Funding Passport.

Surse: MIPE / SMIS / data.gov.ro, în funcție de disponibilitatea și licența dataseturilor.

### 4.7 Explicit out of scope

- SEAP / public procurement intelligence;
- generic people search;
- anonymous lookup pe persoane fizice;
- black-box AI risk scoring.

---

## 5. Catalogul inițial de servicii

### Company

| Cod | Produs | Tip | MVP |
|---|---|---|---|
| `EV-COMP-001` | Verificare firmă | INSTANT_CHECK | Da |
| `EV-FISC-001` | Verificare TVA | INSTANT_CHECK | Da |
| `EV-FISC-002` | Verificare RO e-Factura | INSTANT_CHECK | Da |
| `EV-FISC-003` | Contribuabil activ/inactiv | INSTANT_CHECK | Da |
| `EV-FISC-004` | Situație financiară | INSTANT_CHECK | Da |
| `EV-FISC-005` | Evoluție financiară | AGGREGATED_REPORT | Da |
| `EV-FISC-006` | Obligații fiscale restante publicate | INSTANT_CHECK | Da |
| `EV-LIT-001` | Verificare litigii | INSTANT_CHECK | Da |
| `EV-LIT-002` | Raport litigii | AGGREGATED_REPORT | Da |
| `EV-COMP-010` | Company Passport | AGGREGATED_REPORT | Da |
| `EV-COMP-020` | Supplier / Customer Risk Passport | AGGREGATED_REPORT | Post-MVP |

### Property

| Cod | Produs | Tip | MVP |
|---|---|---|---|
| `EV-PROP-001` | Extras CF | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-002` | Plan cadastral / ortofoto | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-003` | Identificare după adresă | PROCEDURE | Post-MVP |
| `EV-PROP-004` | Identificare după proprietar | PROCEDURE | Restricționat |
| `EV-PROP-005` | Actualizare adresă CF | PROCEDURE | Post-MVP |
| `EV-PROP-006` | Notare autorizație construire | PROCEDURE | Post-MVP |
| `EV-PROP-007` | Înscriere construcție | PROCEDURE | Post-MVP |
| `EV-PROP-008` | Plan încadrare Ilfov | OFFICIAL_DOCUMENT | Post-MVP |
| `EV-PROP-009` | Plan 1:500 / 1:2000 București | OFFICIAL_DOCUMENT | Post-MVP |
| `EV-PROP-010` | CF extenso | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-011` | Încheiere intabulare | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-012` | PAD | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-013` | Releveu | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-014` | Act proprietate | OFFICIAL_DOCUMENT | Da |
| `EV-PROP-020` | Property Passport | AGGREGATED_REPORT | Post-MVP |

### Licensing

| Cod | Produs | Tip | MVP |
|---|---|---|---|
| `EV-LIC-001` | Verificare ANRE gaze | INSTANT_CHECK | Da |
| `EV-LIC-002` | Verificare ANRE electricitate | INSTANT_CHECK | Da |
| `EV-LIC-010` | Contractor Passport | AGGREGATED_REPORT | Post-MVP |

### Transport

| Cod | Produs | Tip | MVP |
|---|---|---|---|
| `EV-TRANS-001` | Verificare transportator | INSTANT_CHECK | După confirmare ARR |
| `EV-TRANS-002` | Transportator Passport | AGGREGATED_REPORT | Post-MVP |

### Monitoring

| Cod | Produs | Tip |
|---|---|---|
| `EV-MON-001` | Company Monitoring | MONITORING |
| `EV-MON-002` | Contractor Monitoring | MONITORING |
| `EV-MON-003` | Transporter Monitoring | MONITORING |

Catalogul detaliat este menținut în [`docs/01-service-catalog.md`](docs/01-service-catalog.md).

---

## 6. Status integrare

Folosim patru stări de maturitate:

- `READY` — există acces programatic oficial utilizabil;
- `DATA_FEED` — există dataset/export oficial machine-readable;
- `CONDITIONAL` — accesul necesită acord, licență sau clarificare;
- `WORKFLOW` — serviciul poate fi livrat, dar presupune proces administrativ/operator.

### Matrix

| Provider | Capabilități | Status |
|---|---|---|
| ANAF | fiscal, TVA, e-Factura, financial | `READY` |
| Justice | dosare/litigii | `READY` |
| ANCPI | property data/document workflows | `CONDITIONAL` + `WORKFLOW` |
| ONRC | company documents | `CONDITIONAL` + `WORKFLOW` |
| ANRE | licenses | `DATA_FEED` |
| ARR | transport licenses | `CONDITIONAL` |
| data.gov.ro / MIPE | open datasets | `READY` / `DATA_FEED` |

Detalii: [`docs/05-integrations.md`](docs/05-integrations.md).

---

## 7. MVP

MVP-ul trebuie să valideze simultan două motoare:

1. **instant checks / reports**;
2. **official document ordering / workflows**.

### In scope

#### Company

- company lookup;
- fiscal status;
- TVA;
- e-Factura;
- active/inactive;
- financial snapshot;
- tax arrears public list;
- litigation;
- Company Passport.

#### Property

- Extras CF;
- cadastral plan;
- CF extenso;
- registration decision;
- PAD;
- floor plan;
- title deed copy.

#### Licensing

- ANRE company authorization checks.

#### Platform

- accounts;
- service catalog;
- checkout;
- orders;
- payments abstraction;
- PDF reports;
- document vault;
- audit;
- admin operations queue;
- provider health.

### Out of scope MVP

- public B2B API;
- complex monitoring;
- EU-VERIFIC Score;
- beneficial owners;
- ARR until access is confirmed;
- ANCPI P3/P4 as hard dependency;
- SEAP.

Vezi [`docs/02-mvp-scope.md`](docs/02-mvp-scope.md).

---

## 8. Tech stack

### Application

- **PHP 8.4**
- **Laravel**
- **Livewire**
- **Blade**
- **PostgreSQL**

### Infrastructure

- **Redis** — queues, locks, cache, rate limiting;
- **S3-compatible object storage** — official documents, generated reports, source artifacts;
- Laravel queue workers;
- scheduled jobs for monitoring/data refresh.

### Architectural style

**Modular monolith.**

Nu folosim microservicii în MVP. Integrarea cu instituțiile este izolată prin provider adapters și capabilities.

---

## 9. Architectural model

Fluxul principal pentru verificări:

```text
SUBJECT
   ↓
CHECK
   ↓
PROVIDER / CAPABILITY
   ↓
CHECK RUN
   ↓
SOURCE SNAPSHOT
   ↓
NORMALIZATION
   ↓
FINDINGS
   ↓
REPORT
```

Fluxul documentelor este separat:

```text
DOCUMENT ORDER
   ↓
REQUIREMENTS / DOCUMENTS / MANDATE
   ↓
PAYMENT
   ↓
WORKFLOW
   ↓
PROVIDER / OPERATOR / AUTHORIZED PARTNER
   ↓
OFFICIAL DOCUMENT
   ↓
DELIVERY
```

Monitoring:

```text
MONITORING SUBSCRIPTION
   ↓
SCHEDULED CHECKS
   ↓
NEW SOURCE SNAPSHOTS
   ↓
CHANGE DETECTION
   ↓
MONITORING EVENT
   ↓
NOTIFICATION
```

---

## 10. Domain boundaries

Aplicația trebuie organizată după domeniu, nu după instituții.

Propunere:

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
│
├── Integrations/
│   ├── Anaf/
│   ├── Justice/
│   ├── Ancpi/
│   ├── Onrc/
│   ├── Anre/
│   ├── Arr/
│   └── DataGov/
│
└── Support/
    ├── Audit/
    ├── Providers/
    ├── RateLimiting/
    └── Observability/
```

---

## 11. Core domain concepts

### Subject

Entitatea verificată.

Tipuri inițiale:

- `Company`
- `Property`
- `LicensedOperator`

### Check

Cererea logică de verificare pentru un subject.

### CheckRun

O execuție efectivă a unei verificări împotriva unui provider.

### SourceSnapshot

Dovada brută a ceea ce a furnizat sursa oficială la un anumit moment.

Trebuie să păstreze cel puțin:

- provider;
- capability;
- request fingerprint;
- raw response / artifact reference;
- `retrieved_at`;
- checksum;
- parser version.

### Finding

Interpretarea deterministică a uneia sau mai multor informații brute.

### Report

Agregarea customer-facing a findings.

### DocumentOrder

Lifecycle-ul comercial și administrativ al unei cereri de document oficial.

### MonitoringSubscription

Definește ce capabilities sunt reverificate și ce schimbări generează evenimente.

Detalii: [`docs/04-domain-model.md`](docs/04-domain-model.md).

---

## 12. Database baseline

Tabele candidate:

```text
users
subjects
companies
properties
licensed_operators

providers
provider_capabilities

checks
check_runs
source_snapshots
findings

reports
report_sections

document_orders
document_order_requirements
documents
mandates
consents

monitoring_subscriptions
monitoring_runs
monitoring_events

orders
order_items
payments
invoices

access_audit_logs
provider_health_checks
```

Schema finală trebuie definită înainte de implementarea Sprint 0.

---

## 13. Provider abstraction

Business logic-ul nu trebuie să depindă direct de SDK-uri sau endpoint-uri instituționale.

Conceptual:

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

Capabilities vor arăta aproximativ astfel:

```text
anaf.fiscal_status
anaf.vat
anaf.efactura
anaf.financials
justice.litigation
anre.company_license
arr.carrier_license
ancpi.land_registry_extract
ancpi.archive_document
```

---

## 14. Async execution

UI-ul nu trebuie să execute în același HTTP request un lanț de servicii externe.

Exemplu Company Passport:

```text
GenerateCompanyPassport
        ↓
 dispatch independent jobs
 ┌──────────┬───────────┬──────────┐
 ↓          ↓           ↓          ↓
ANAF      Justice     ANRE      Other
 ↓          ↓           ↓
SourceSnapshots
        ↓
Findings
        ↓
CompileReport
        ↓
REPORT_READY
```

Necesare:

- idempotency;
- retries;
- exponential backoff;
- provider-specific rate limiting;
- dead-letter / failed jobs handling;
- reconciliation;
- manual retry/admin takeover.

---

## 15. Workflow states

### Check

```text
PENDING
→ QUEUED
→ RUNNING
→ READY
```

Alternative:

- `PARTIAL`
- `FAILED`
- `CANCELLED`

### Document order

```text
DRAFT
→ AWAITING_DOCUMENTS
→ AWAITING_MANDATE
→ READY_FOR_PAYMENT
→ PAID
→ READY_FOR_SUBMISSION
→ SUBMITTED
→ PROVIDER_PROCESSING
→ DOCUMENT_RECEIVED
→ DELIVERED
```

Alternative:

- `CUSTOMER_ACTION_REQUIRED`
- `COMPLETION_REQUESTED`
- `REJECTED`
- `CANCELLED`

Vezi [`docs/06-workflows.md`](docs/06-workflows.md).

---

## 16. Audit & compliance

Audit-ul este o cerință de produs, nu doar infrastructură.

Pentru fiecare acces relevant trebuie să putem demonstra:

```text
who
what
subject
provider
capability
purpose
legal basis / mandate
when
request fingerprint
result reference
```

Principii:

- least privilege;
- purpose limitation;
- explicit retention;
- encryption in transit și, unde este aplicabil, at rest;
- separarea raw evidence de interpretare;
- fără anonymous sensitive-person lookup;
- access control pentru providers restricționați.

Vezi [`docs/07-compliance-security.md`](docs/07-compliance-security.md).

---

## 17. Roadmap de development

### Sprint 0 — Platform Core

- Laravel bootstrap;
- PostgreSQL;
- Redis;
- auth;
- subjects;
- providers/capabilities;
- checks/check runs;
- source snapshots;
- findings;
- audit;
- queues;
- storage abstraction;
- billing abstraction;
- baseline tests.

### Sprint 1 — ANAF

- company lookup;
- fiscal status;
- TVA;
- e-Factura;
- financials;
- provider rate limiting;
- normalization;
- source snapshots;
- integration tests.

### Sprint 2 — Justice + Company Passport

- litigation provider;
- company name matching;
- findings;
- report engine;
- Company Passport UI.

### Sprint 3 — Commerce

- service catalog;
- orders;
- checkout;
- payment provider;
- invoices;
- PDF reports;
- customer account/history.

### Sprint 4 — ANCPI workflows

- document orders;
- dynamic requirements;
- mandates;
- back-office queue;
- document vault;
- submission/provider abstraction.

### Sprint 5 — ANRE + Monitoring Foundation

- ANRE data synchronization;
- contractor checks;
- change detection;
- monitoring subscriptions;
- notifications baseline.

Detalii: [`docs/08-roadmap.md`](docs/08-roadmap.md).

---

## 18. Definition of Done pentru o integrare

O integrare nu este considerată implementată doar pentru că răspunde un endpoint.

Pentru fiecare provider/capability trebuie să existe:

- sursă oficială documentată;
- drept/condiții de acces clarificate;
- request/response contract;
- timeout;
- retry policy;
- rate limit policy;
- caching/freshness policy;
- source snapshot policy;
- normalization;
- failure mapping;
- tests cu fixtures;
- observability;
- audit behavior;
- fallback/manual procedure, dacă este necesar.

---

## 19. Documentație

| Document | Scop |
|---|---|
| [`docs/00-product-overview.md`](docs/00-product-overview.md) | produs și poziționare |
| [`docs/01-service-catalog.md`](docs/01-service-catalog.md) | catalogul SKU/capabilities |
| [`docs/02-mvp-scope.md`](docs/02-mvp-scope.md) | scope MVP |
| [`docs/03-architecture.md`](docs/03-architecture.md) | arhitectura aplicației |
| [`docs/04-domain-model.md`](docs/04-domain-model.md) | domain model |
| [`docs/05-integrations.md`](docs/05-integrations.md) | provider matrix |
| [`docs/06-workflows.md`](docs/06-workflows.md) | state machines |
| [`docs/07-compliance-security.md`](docs/07-compliance-security.md) | security/privacy/audit |
| [`docs/08-roadmap.md`](docs/08-roadmap.md) | roadmap |
| [`docs/09-open-questions.md`](docs/09-open-questions.md) | întrebări nerezolvate |
| [`docs/integrations/`](docs/integrations/) | specificații per instituție |
| [`docs/adr/`](docs/adr/) | architecture decision records |

---

## 20. Decizii actuale

- Brand: **EU-VERIFIC**.
- Domain: **eu-verific.ro**.
- Repository: **iCheck**.
- Stack: **PHP 8.4 + Laravel + Livewire + Blade + PostgreSQL**.
- Redis este recomandat pentru queues/cache/locks/rate limiting.
- Storage pentru documente: S3-compatible.
- Architecture: **modular monolith**.
- SEAP: exclus din produs.
- Provider integrations: async-first.
- Raw source evidence se păstrează separat de findings.
- Scoring black-box: exclus.
- API B2B: proiectat intern, expus public post-MVP.

---

## 21. Următorii pași

1. Definitivarea catalogului complet de servicii.
2. Specificația tehnică a MVP-ului.
3. Schema PostgreSQL și migrations plan.
4. Domain model și enums.
5. Provider contracts.
6. ANAF integration specification.
7. Justice integration specification.
8. Bootstrap Laravel și Sprint 0.

Acest repository trebuie să devină **source of truth** atât pentru produs, cât și pentru implementare.
