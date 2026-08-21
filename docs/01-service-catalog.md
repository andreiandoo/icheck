# Service Catalog

Acest document este registrul produselor EU-VERIFIC. Fiecare serviciu trebuie definit ca SKU comercial și capability tehnică.

## Schema obligatorie pentru un serviciu

- `code`
- `name`
- `vertical`
- `product_type`
- `customer_problem`
- `inputs`
- `outputs`
- `official_source`
- `provider_capability`
- `integration_status`
- `legal_constraints`
- `official_cost`
- `selling_price_hypothesis`
- `automation_level`
- `freshness_requirement`
- `sla_hypothesis`
- `mvp_status`
- `fallback`

## Integration statuses

- `READY`
- `DATA_FEED`
- `CONDITIONAL`
- `WORKFLOW`

## Company

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-COMP-001 | Verificare firmă | INSTANT_CHECK | ANAF + core registry data | READY | Da |
| EV-FISC-001 | Verificare TVA | INSTANT_CHECK | ANAF | READY | Da |
| EV-FISC-002 | RO e-Factura | INSTANT_CHECK | ANAF | READY | Da |
| EV-FISC-003 | Activ/inactiv fiscal | INSTANT_CHECK | ANAF | READY | Da |
| EV-FISC-004 | Situație financiară | INSTANT_CHECK | ANAF/MF | READY | Da |
| EV-FISC-005 | Evoluție financiară | AGGREGATED_REPORT | EU-VERIFIC | READY | Da |
| EV-FISC-006 | Restanțe fiscale publicate | INSTANT_CHECK | ANAF | DATA_FEED | Da |
| EV-LIT-001 | Verificare litigii | INSTANT_CHECK | Ministerul Justiției | READY | Da |
| EV-LIT-002 | Raport litigii | AGGREGATED_REPORT | EU-VERIFIC | READY | Da |
| EV-COMP-010 | Company Passport | AGGREGATED_REPORT | Multiple | READY | Da |
| EV-COMP-020 | Supplier / Customer Risk Passport | AGGREGATED_REPORT | Multiple | READY | Post-MVP |

## Property

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-PROP-001 | Extras CF | OFFICIAL_DOCUMENT | ANCPI | WORKFLOW | Da |
| EV-PROP-002 | Plan cadastral / ortofoto | OFFICIAL_DOCUMENT | ANCPI | WORKFLOW | Da |
| EV-PROP-003 | Identificare după adresă | PROCEDURE | ANCPI/OCPI | WORKFLOW | Post-MVP |
| EV-PROP-004 | Identificare după proprietar | PROCEDURE | ANCPI | CONDITIONAL | Restricționat |
| EV-PROP-005 | Actualizare adresă CF | PROCEDURE | OCPI | WORKFLOW | Post-MVP |
| EV-PROP-006 | Notare autorizație construire | PROCEDURE | OCPI | WORKFLOW | Post-MVP |
| EV-PROP-007 | Înscriere construcție | PROCEDURE | OCPI + autorizat ANCPI | WORKFLOW | Post-MVP |
| EV-PROP-008 | Plan încadrare Ilfov | OFFICIAL_DOCUMENT | OCPI Ilfov | WORKFLOW | Post-MVP |
| EV-PROP-009 | Plan 1:500 / 1:2000 București | OFFICIAL_DOCUMENT | OCPI București | WORKFLOW | Post-MVP |
| EV-PROP-010 | Copie CF extenso | OFFICIAL_DOCUMENT | ANCPI/OCPI | WORKFLOW | Da |
| EV-PROP-011 | Copie încheiere intabulare | OFFICIAL_DOCUMENT | ANCPI/OCPI | WORKFLOW | Da |
| EV-PROP-012 | Copie PAD | OFFICIAL_DOCUMENT | ANCPI/OCPI | WORKFLOW | Da |
| EV-PROP-013 | Copie releveu | OFFICIAL_DOCUMENT | ANCPI/OCPI | WORKFLOW | Da |
| EV-PROP-014 | Copie act proprietate | OFFICIAL_DOCUMENT | ANCPI/OCPI | WORKFLOW | Da |
| EV-PROP-020 | Property Passport | AGGREGATED_REPORT | ANCPI + EU-VERIFIC | CONDITIONAL | Post-MVP |

## Licensing

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-LIC-001 | Verificare ANRE gaze | INSTANT_CHECK | ANRE | DATA_FEED | Da |
| EV-LIC-002 | Verificare ANRE electricitate | INSTANT_CHECK | ANRE | DATA_FEED | Da |
| EV-LIC-003 | Verificare electrician autorizat | INSTANT_CHECK | ANRE | DATA_FEED | Post-MVP |
| EV-LIC-010 | Contractor Passport | AGGREGATED_REPORT | Multiple | DATA_FEED | Post-MVP |

## Transport

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-TRANS-001 | Verificare transportator | INSTANT_CHECK | ARR | CONDITIONAL | După confirmare |
| EV-TRANS-002 | Transportator Passport | AGGREGATED_REPORT | ARR + ANAF + Justice | CONDITIONAL | Post-MVP |

## ONRC Documents

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-ONRC-001 | Certificat constatator | OFFICIAL_DOCUMENT | ONRC | CONDITIONAL/WORKFLOW | Post-MVP |
| EV-ONRC-002 | Furnizare informații ONRC | OFFICIAL_DOCUMENT | ONRC | CONDITIONAL/WORKFLOW | Post-MVP |
| EV-ONRC-003 | Istoric societate | OFFICIAL_DOCUMENT | ONRC | CONDITIONAL/WORKFLOW | Post-MVP |
| EV-ONRC-004 | Beneficiar real | OFFICIAL_DOCUMENT | ONRC | RESTRICTED | Nu MVP |

## Funding

| Code | Name | Type | Source | Status | MVP |
|---|---|---|---|---|---|
| EV-GRANT-001 | Verificare finanțări UE | INSTANT_CHECK | MIPE/data.gov.ro | DATA_FEED | Post-MVP |
| EV-GRANT-002 | Funding Passport | AGGREGATED_REPORT | MIPE/data.gov.ro | DATA_FEED | Post-MVP |

## Monitoring

| Code | Name | Type | Monitored capabilities |
|---|---|---|---|
| EV-MON-001 | Company Monitoring | MONITORING | fiscal, TVA, e-Factura, financials, arrears, litigation |
| EV-MON-002 | Contractor Monitoring | MONITORING | company + ANRE licenses |
| EV-MON-003 | Transporter Monitoring | MONITORING | company + ARR license |

## Pricing hypotheses

Pricing-ul este ipoteză până la validare comercială.

- verificări simple: 9–29 lei;
- vertical checks: 29–49 lei;
- Company Passport: 49–69 lei;
- risk reports: 79–119 lei;
- Property Passport: 99–199 lei;
- documente oficiale: official cost + service margin;
- monitoring: subscription tiers.

## Definition of Done per SKU

Un SKU poate fi marcat `ACTIVE` doar dacă avem:

1. sursa și accesul clarificate;
2. inputs/outputs definite;
3. legal constraints documentate;
4. pricing/cost model;
5. provider implementation sau workflow manual;
6. fallback;
7. customer-facing wording;
8. audit și source evidence;
9. tests;
10. observability.
