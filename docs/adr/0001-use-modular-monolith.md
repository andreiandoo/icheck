# ADR 0001 — Use a modular monolith

- **Status:** ACCEPTED
- **Date:** 2026-08-21

## Context

EU-VERIFIC spans several business domains and external providers, but the MVP is developed by a small team and needs fast iteration, simple deployment and strong transactional consistency.

Introducing microservices at this stage would add deployment, observability, networking, contract-versioning and operational complexity without solving a demonstrated scaling problem.

## Decision

Build EU-VERIFIC as a Laravel modular monolith.

Domain boundaries are explicit in code and documentation. Provider integrations are adapters isolated from domain logic.

## Alternatives considered

### Microservices per provider

Rejected because providers are technical integrations, not business domains, and this would create unnecessary operational coupling.

### Microservices per business domain

Deferred until independent scaling/deployment needs are demonstrated.

### Unstructured traditional Laravel app

Rejected because it would encourage provider logic, controllers and Eloquent models to become tightly coupled as the catalog grows.

## Consequences

Positive:

- simple local development/deployment;
- easy cross-domain transactions;
- lower operational cost;
- refactoring remains possible;
- clear boundaries can later be extracted if necessary.

Negative:

- architectural discipline must be enforced in one codebase;
- modules are not physically isolated processes;
- careless direct model dependencies can erode boundaries.

## Review trigger

Revisit when a domain requires independent scaling, release cadence, ownership or security isolation that the monolith cannot reasonably provide.
