# Compliance & Security

## Scope

EU-VERIFIC processes official-source data, customer identity/account data, documents and potentially restricted personal/property information. Compliance must be designed into the product from the start.

This document is an engineering baseline, not a substitute for legal review.

## Core principles

- least privilege;
- purpose limitation;
- data minimization;
- explicit retention;
- auditable access;
- secure secrets handling;
- source traceability;
- separation of raw evidence from customer-facing interpretation;
- no anonymous sensitive-person lookup;
- provider-specific legal access controls.

## Data classification

Suggested classes:

### PUBLIC

Data clearly published as open/public information, subject to source terms and freshness.

### CUSTOMER_CONFIDENTIAL

Account, orders, invoices, uploaded files, reports purchased by the customer.

### RESTRICTED_PERSONAL

Personal/property/beneficial-owner data requiring purpose/legal basis or provider authorization.

### SECRET

API credentials, OAuth secrets, certificates, private keys.

Every table/file category should have an assigned class before production.

## Authentication & authorization

Initial roles:

- customer;
- organization member;
- organization admin;
- operations agent;
- compliance/admin;
- system/service account.

Use Laravel policies for ownership/resource authorization.

Operations users must not automatically receive unrestricted access to all sensitive source data. Implement role/capability gates.

## Audit

At minimum record:

```text
actor
actor role
subject
provider
capability
action
purpose
legal basis / mandate reference
request fingerprint
resource/result reference
timestamp
```

Audit records should be append-only in normal application flows.

## Restricted checks

Before dispatching a restricted capability, application services must validate required preconditions, e.g.:

- authenticated requester;
- stated purpose;
- mandate/authorization;
- legitimate-interest evidence where applicable;
- provider access permission.

Do not rely on hiding a UI button as an access control.

## Source snapshots

Raw provider data may contain more information than the final report. Access to raw snapshots should be stricter than access to customer-facing normalized findings.

Production logs must not serialize full provider payloads by default.

## Documents

Official/customer documents:

- private object storage only;
- signed short-lived access URLs or streamed authorized downloads;
- checksum on upload/receipt;
- malware scanning for customer uploads where practical;
- MIME/type validation;
- no public bucket paths;
- explicit retention policy.

## Encryption

- TLS for all external/internal network access;
- database/storage encryption according to infrastructure capabilities;
- application-level encryption for especially sensitive provider credentials/fields;
- Laravel encrypted casts/secret management where appropriate.

## Secrets

Never commit:

- `.env`;
- API secrets;
- OAuth client secrets;
- certificates/private keys;
- production tokens.

Use environment/secret manager injection.

## Logging

Do log:

- provider;
- capability;
- request correlation ID;
- timings;
- normalized error category;
- HTTP status where safe;
- retry attempt.

Do not log by default:

- full CNPs;
- identity documents;
- passwords/tokens;
- provider raw payloads containing personal data;
- uploaded document contents.

## Rate limiting / abuse

Customer-facing lookup endpoints need abuse controls even if upstream sources allow high traffic.

Potential controls:

- IP/account throttles;
- authentication for higher-volume use;
- product-specific quotas;
- bot detection if abuse appears;
- deny bulk enumeration of restricted subjects.

## Data retention

Retention must be defined by data category/product.

Questions to answer before production:

- how long do we retain source snapshots?
- how long are purchased reports accessible?
- how long do we retain mandates?
- how long do we retain official documents?
- what must be retained for accounting/legal defense?
- how do deletion requests interact with legal retention?

Do not implement one global delete-after-X-days rule.

## GDPR / DPIA

A dedicated privacy review/DPIA is required before exposing workflows involving:

- owner searches;
- beneficial owners;
- broad person-based litigation searches;
- identity documents;
- continuous monitoring of natural persons.

Company data can still contain personal data (administrators, representatives etc.), so B2B does not eliminate GDPR obligations.

## Customer-facing disclaimers

Reports must distinguish:

- official facts;
- EU-VERIFIC-calculated metrics;
- EU-VERIFIC findings;
- unavailable/stale data.

Avoid representing a commercial risk indicator as a legal, credit-rating or professional certification unless the product has the required basis/authorization.

## Incident readiness

Before public launch define:

- provider credential compromise procedure;
- customer document exposure procedure;
- data breach triage;
- audit export capability;
- ability to revoke sessions/tokens;
- provider connector kill switch.

## Compliance backlog

- privacy policy and terms model;
- controller/processor mapping by workflow;
- data inventory;
- retention matrix;
- DPIA where required;
- provider terms/license register;
- subprocessor register;
- DPA templates for B2B customers where applicable.
