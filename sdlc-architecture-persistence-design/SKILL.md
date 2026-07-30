---
name: sdlc-architecture-persistence-design
description: Design persistence architecture for SDLC projects. Use when choosing databases or object stores, moving from in-memory stores to real persistence, designing schemas, repositories, migrations, audit/history, transaction boundaries, retention, or deciding what belongs in storage versus workflow/domain logic.
---

# SDLC Architecture Persistence Design

Use this skill before persistence becomes load-bearing or when an in-memory implementation is starting to own business rules.

## Core Question

Separate these responsibilities deliberately:

- **Domain/workflow rules**: validation, lifecycle transitions, authorization policy, eligibility, calculations, and orchestration.
- **Repository/data access**: load, save, query, transaction, uniqueness, concurrency, and persistence errors.
- **Schema/storage**: durable shape, indexes, constraints, migrations, retention, and audit/history records.

Do not let "we need a database" turn into "the database adapter owns the domain".

## Inputs

Inspect:

- domain model and glossary;
- requirements and acceptance criteria;
- ADRs and codebase-design notes;
- current in-memory stores and repositories;
- workflow/application services;
- contracts/events;
- tests that rely on persistence behaviour;
- privacy, consent, provenance, retention, and audit requirements.

## Design Process

1. Identify aggregates and lifecycle states that need durable storage.
2. Identify invariants that belong in workflow/domain code.
3. Identify invariants that also need database constraints.
4. Define repository protocols around use-case needs, not table shape.
5. Decide transaction boundaries and consistency expectations.
6. Decide migration strategy before the first durable schema ships.
7. Decide audit/history requirements explicitly.
8. Define test seams: workflow tests, repository contract tests, migration tests, and integration tests.
9. Record material technology choices or trade-offs in ADRs.

## Repository Guidance

- Prefer persistence-shaped methods: `get`, `save`, `list`, `find`, `delete`, `exists`.
- Avoid use-case-shaped repository methods such as `approve_review`, `assign_dataset_role`, or `create_training_export`; those belong in workflows.
- Keep protocols small and grouped by aggregate or workflow need.
- Introduce protocols where a real adapter is near-term or a boundary is being clarified.
- Use in-memory repositories as test adapters, not as the only place behaviour can live.

## Schema Guidance

Capture:

- identifiers and human-readable keys;
- ownership and tenant/workspace relationships;
- lifecycle state fields;
- timestamps and actors;
- provenance and source references;
- uniqueness and idempotency constraints;
- soft-delete, archive, or retention rules;
- data classification and privacy constraints.

## Review Checks

- Can the in-memory adapter be replaced without rewriting business rules?
- Are workflow/domain rules testable without a real database?
- Are database constraints backing the invariants that must survive concurrency?
- Are migrations and rollback expectations named?
- Is provenance/audit/history treated as first-class when required?
- Are open storage decisions recorded as ADR follow-ons or parked items?

## Closeout

Update relevant architecture docs, ADRs, parking-lot items, and AI-SDLC observations when persistence decisions or deferred storage work change.
