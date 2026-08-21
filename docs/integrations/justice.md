# Justice Integration

Status: **READY / MVP CORE**

## Scope

Ministerul Justiției / portal.just.ro provides the litigation source used by company checks and Company Passport.

Initial capability:

```text
justice.litigation
```

## Expected query modes

The official service supports queries around:

- case number;
- party name;
- subject/object;
- court;
- date/period.

For EU-VERIFIC the main lookup key is company legal name, therefore entity matching is a core problem.

## Matching strategy

Do not assume exact party-name equality is sufficient.

Required matching pipeline:

1. canonical company legal name;
2. normalized diacritics/punctuation/spacing;
3. common legal-form normalization (`S.R.L.`, `SRL`, etc.);
4. historical names when available;
5. preserve ambiguous matches rather than silently treating them as confirmed.

Each case association should have a match confidence/method.

## Normalized case model

```text
provider_case_id
case_number
court
case_object
stage
filed_at nullable
last_hearing_at nullable
next_hearing_at nullable
parties[]
appeals[]
source_updated_at nullable
```

## Findings

Examples:

- `ACTIVE_LITIGATION`
- `INSOLVENCY_CASE`
- `PAYMENT_ORDER_CASE`
- `COMMERCIAL_CLAIM_CASE`
- `EMPLOYMENT_LITIGATION`

Presence in a case is not inherently negative. Rules must consider role, object and lifecycle where reliably available.

## Source snapshots

Persist raw service response/reference and retrieval timestamp. If a response can be large, store canonical raw representation in object storage or compressed JSON according to retention policy.

## Freshness

Litigation should be treated as changing data. Customer report must display when the search was performed. Monitoring can later re-run the same normalized query and compare canonical case identifiers/status.

## Failure modes

- provider unavailable;
- timeout;
- malformed response;
- ambiguous company name;
- no results;
- provider schema change.

`NO_RESULTS` is a valid result, not a provider failure.

## Tests

- exact company match;
- company with multiple cases;
- legal-form variants;
- ambiguous same/similar names;
- no cases;
- provider timeout;
- malformed response;
- duplicate result normalization.

## Open questions

See `docs/09-open-questions.md` for rate-limit, identifier and matching questions.
