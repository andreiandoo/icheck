# ADR 0002 — Preserve source snapshots as evidence

- **Status:** ACCEPTED
- **Date:** 2026-08-21

## Context

EU-VERIFIC transforms official-source data into normalized facts, findings and reports. Provider data can change over time, parser logic can evolve and customers may need to understand what information supported a paid result at the moment it was produced.

Storing only the final interpreted result would make historical verification, debugging and dispute handling difficult.

## Decision

Persist an immutable `SourceSnapshot` for provider evidence used in checks/reports, subject to provider terms, privacy requirements and data minimization rules.

A snapshot includes at minimum:

- provider;
- capability;
- request fingerprint/metadata;
- retrieval timestamp;
- raw payload or artifact reference where permitted;
- checksum;
- parser version.

Findings reference the evidence from which they were derived.

## Alternatives considered

### Store only normalized data

Rejected because provider responses and interpretation cannot be reconstructed reliably after schema/parser changes.

### Store provider responses only in application logs

Rejected because logs are unsuitable as durable business evidence, can be rotated and should avoid sensitive payloads.

### Re-query provider when evidence is needed

Rejected because the provider may return different data later and historical state would be lost.

## Consequences

Positive:

- strong traceability;
- easier debugging;
- reproducible reports;
- parser/rule reprocessing is possible;
- clearer audit trail.

Negative:

- additional storage;
- retention/privacy complexity;
- source licenses may restrict what can be retained;
- raw evidence requires stricter access controls.

## Constraints

This ADR does not require retaining every byte forever. Provider-specific retention and minimization policies can limit snapshot content while preserving adequate provenance.
