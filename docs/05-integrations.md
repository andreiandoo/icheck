# Integrations

## Integration maturity states

- `READY` — există acces programatic oficial utilizabil;
- `DATA_FEED` — există dataset/export oficial machine-readable;
- `CONDITIONAL` — accesul necesită acord, licență sau clarificare;
- `WORKFLOW` — serviciul este livrabil, dar presupune proces administrativ/operator.

## Provider matrix

| Provider | Main capabilities | Current status | MVP role |
|---|---|---|---|
| ANAF | fiscal, TVA, e-Factura, financial | READY | Core |
| Justice | litigation | READY | Core |
| ANCPI | property data/documents/workflows | CONDITIONAL + WORKFLOW | Core document workflow |
| ONRC | company documents | CONDITIONAL + WORKFLOW | Post-MVP unless access clarified |
| ANRE | licenses | DATA_FEED | MVP if sync stable |
| ARR | carrier licenses | CONDITIONAL | Post-confirmation |
| data.gov.ro / MIPE | open datasets / funding | READY / DATA_FEED | Post-MVP |

## Integration contract checklist

Every provider integration must document:

1. official source and legal basis;
2. commercial/reuse conditions;
3. authentication;
4. transport protocol;
5. endpoints/feeds/files;
6. request contract;
7. response contract;
8. provider identifiers;
9. rate limits;
10. timeouts;
11. retry policy;
12. caching/freshness;
13. normalization;
14. source snapshot policy;
15. failure mapping;
16. observability;
17. integration tests/fixtures;
18. manual fallback;
19. unresolved questions.

## Provider capabilities are explicit

Do not expose generic methods such as `AnafClient::request($path)` to domain logic. Define capabilities with clear semantics.

Examples:

```text
anaf.vat
anaf.efactura
anaf.fiscal_status
anaf.financials
justice.litigation
anre.company_license
ancpi.land_registry_extract
ancpi.cadastral_plan
ancpi.archive_document
```

## Data freshness

Each capability must classify data as one of:

- live/near-live lookup;
- periodically updated registry;
- published snapshot/list;
- historical dataset;
- official document valid at issue time.

The customer-facing wording must match the freshness model.

## Failure categories

Normalize provider errors into internal categories:

```text
PROVIDER_UNAVAILABLE
PROVIDER_TIMEOUT
RATE_LIMITED
AUTH_FAILED
INVALID_REQUEST
NOT_FOUND
AMBIGUOUS_MATCH
ACCESS_DENIED
LEGAL_BASIS_REQUIRED
MANUAL_PROCESSING_REQUIRED
PROVIDER_RESPONSE_INVALID
UNKNOWN_PROVIDER_ERROR
```

Provider-specific codes belong in error metadata, not in application business branching.

## Documentation

- [`integrations/anaf.md`](integrations/anaf.md)
- [`integrations/justice.md`](integrations/justice.md)
- [`integrations/ancpi.md`](integrations/ancpi.md)
- [`integrations/onrc.md`](integrations/onrc.md)
- [`integrations/anre.md`](integrations/anre.md)
- [`integrations/arr.md`](integrations/arr.md)
- [`integrations/datagov.md`](integrations/datagov.md)
