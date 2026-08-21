# ONRC Integration

Status: **CONDITIONAL / WORKFLOW**

## Scope

ONRC is relevant for official company documents and enhanced company identity/history.

Potential capabilities:

```text
onrc.company_certificate
onrc.company_information
onrc.company_history
onrc.beneficial_owner
```

## Current strategy

Do not make ONRC a hard MVP dependency until commercial machine-to-machine access is clarified.

The product architecture should support both:

- manual/operational ordering via official channels;
- future M2M adapter if ONRC provides/approves commercial access.

## Products

- `EV-ONRC-001` Certificat constatator
- `EV-ONRC-002` Furnizare informații
- `EV-ONRC-003` Istoric societate
- `EV-ONRC-004` Beneficiar real — restricted/not MVP

## Beneficial owners

This capability is legally sensitive and should not be implemented as anonymous lookup.

Before implementation require:

- legal review;
- legitimate-interest workflow where applicable;
- authenticated requester;
- supporting evidence;
- approval state;
- full audit.

## Document workflow

```text
DocumentOrder
→ company identification
→ requester information
→ supporting requirements
→ payment
→ ONRC submission/manual workflow
→ official document
→ private document vault
→ delivery
```

## Open questions

- commercial M2M InfoCert access;
- ordering on behalf of customers;
- pricing/contract;
- sandbox/test environment;
- document status polling/webhooks;
- reuse rights for returned company data;
- beneficial-owner legitimate-interest handling.
