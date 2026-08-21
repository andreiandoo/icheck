# ADR 0003 — Execute external provider integrations asynchronously

- **Status:** ACCEPTED
- **Date:** 2026-08-21

## Context

EU-VERIFIC depends on external state systems with different performance, maintenance windows, rate limits and reliability characteristics. A customer report can require several providers in parallel.

Executing all provider calls inline in the web request would make UX and order fulfillment depend directly on upstream latency/availability.

## Decision

External provider execution is async-first using Laravel queues and Redis-backed coordination.

The web/application layer creates checks/orders and dispatches provider jobs. Results are persisted and reflected in Livewire UI through status polling/events.

Simple synchronous lookups may be allowed later as an explicit exception when proven safe, but async is the default architecture.

## Required mechanics

- idempotent jobs;
- provider-specific rate limiting;
- timeouts;
- retry/backoff;
- normalized failures;
- failed-job visibility;
- reconciliation/manual retry;
- correlation IDs.

## Alternatives considered

### Synchronous provider calls from Livewire/controllers

Rejected as the default because one degraded provider can exhaust PHP workers and create customer-facing timeouts.

### Dedicated microservice queue per institution

Rejected for MVP as unnecessary infrastructure complexity.

## Consequences

Positive:

- resilient customer flows;
- parallel provider execution;
- controlled retries/rate limits;
- provider downtime does not lose orders;
- better observability.

Negative:

- eventual consistency in UI;
- more explicit state machines;
- queue infrastructure is mandatory;
- developers must design jobs for retries/idempotency.

## Operational implication

Redis and queue workers are first-class production dependencies, not optional later optimizations.
