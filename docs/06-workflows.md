# Workflows

## Check lifecycle

```text
PENDING
→ QUEUED
→ RUNNING
→ READY
```

Alternative terminal/intermediate states:

- `PARTIAL`
- `FAILED`
- `CANCELLED`

### Invariants

- `READY` means all mandatory capabilities are resolved;
- `PARTIAL` means at least one required capability failed/unavailable but a usable report can be produced;
- retries create new `CheckRun` attempts, not duplicate customer checks;
- provider failures are mapped to normalized internal error categories.

## CheckRun lifecycle

```text
PENDING
→ DISPATCHED
→ RUNNING
→ SUCCEEDED
```

Alternative states:

- `RETRY_SCHEDULED`
- `FAILED`
- `CANCELLED`

Track attempt count, last provider error and next retry time.

## Report lifecycle

```text
DRAFT
→ COLLECTING_DATA
→ COMPILING
→ READY
```

Alternative:

- `PARTIAL`
- `FAILED`
- `EXPIRED`

A report should never parse provider payloads directly; compilation consumes normalized findings/snapshots.

## Document order lifecycle

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

Alternative states:

- `CUSTOMER_ACTION_REQUIRED`
- `COMPLETION_REQUESTED`
- `PROVIDER_DELAYED`
- `REJECTED`
- `CANCELLED`
- `REFUND_PENDING`
- `REFUNDED`

## Document order invariants

1. Submission requires all mandatory requirements complete.
2. Submission requires payment unless explicitly configured otherwise.
3. Restricted products require mandate/legal basis before submission.
4. Retrying submission must use an idempotency guard.
5. Provider reference is stored immediately when available.
6. Final official document is immutable after receipt.
7. Customer delivery creates a delivery event/audit record.

## Archive-document workflow

Products such as PAD, releveu, act proprietate and registration decision should share one backend workflow:

```text
ArchiveDocumentRequest
  document_type
  property
  requester
  requester_capacity
  justification
  supporting_documents
  mandate
```

Frontend SKUs remain separate for clarity; backend orchestration is shared.

## Company Passport workflow

```text
Customer requests Company Passport
→ resolve company identity
→ create report
→ calculate required capabilities
→ dispatch provider checks
→ persist source snapshots
→ normalize results
→ produce findings
→ compile sections
→ mark READY/PARTIAL
→ generate PDF on demand/background
```

## Monitoring lifecycle

```text
ACTIVE
→ SCHEDULED
→ RUNNING
→ ACTIVE
```

Alternative:

- `PAUSED`
- `FAILED`
- `CANCELLED`

Monitoring run:

```text
load previous canonical snapshot
→ run capabilities
→ store new snapshots
→ compare material fields/findings
→ create monitoring events
→ notification policy
```

## Change detection

Do not notify for every raw payload difference. Changes need semantic rules.

Examples:

- VAT `active → inactive`: material;
- new insolvency case: material;
- legal name whitespace difference: not material;
- ANRE license expiry date changed: material;
- new annual financial statement: material but severity depends on findings.

## Payment/order lifecycle

Commercial `Order` state should be separate from operational `Check`/`DocumentOrder` state.

Suggested:

```text
DRAFT
→ PENDING_PAYMENT
→ PAID
→ FULFILLING
→ COMPLETED
```

Alternative:

- `PAYMENT_FAILED`
- `CANCELLED`
- `PARTIALLY_REFUNDED`
- `REFUNDED`

One order may contain several items creating different fulfillment aggregates.

## Notifications

Events that may trigger notifications:

- payment confirmed;
- report ready;
- report partial with delayed source;
- customer action required;
- document submitted;
- completion request received;
- document ready;
- monitoring material change;
- provider delay only when customer SLA is affected.

## Admin takeover

Operational UI must support:

- retry failed check run;
- mark provider as manual-only;
- attach provider reference;
- request customer document/action;
- upload official result;
- add internal operational notes;
- see full event/audit timeline.
