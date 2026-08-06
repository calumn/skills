---
name: sdlc-delivery-vertical-slice-planning
description: Define implementation-ready vertical slices. Use when turning requirements, product specs, architecture notes, or a feature idea into a thin end-to-end slice that is demoable, testable, and ready for TDD implementation.
---

# SDLC Delivery Vertical Slice Planning

Use this skill when the user wants to define the next buildable slice, break a spec into tracer-bullet work, or prepare an implementation plan that cuts through the stack.

This skill is adapted from Matt Pocock's `to-tickets` skill. Keep the tracer-bullet discipline, but publish local planning artifacts unless the user explicitly asks for issue tracker tickets.

## Inputs

Read the smallest set of current project artifacts needed to understand the feature:

- `CONTEXT.md` for canonical domain language.
- Relevant requirements, product spec, Gherkin scenarios, acceptance criteria, and decision log entries.
- Relevant architecture docs and ADRs.
- `architecture/parking-lot.md` when present, to check whether parked work is relevant to the new slice.
- Existing code only enough to understand current seams, services, and test surfaces.

Use project vocabulary exactly. If the language conflicts with `CONTEXT.md`, pause and resolve the term before writing the slice.

## Vertical Slice Rules

A vertical slice is a narrow but complete path through the product.

- Do not assign slice numbers to future candidate work. Keep roadmap candidates named descriptively until the work is promoted into an actual slice artifact or explicitly scheduled into the delivery order.
- It must be demoable or verifiable on its own.
- It should cut through the relevant layers: data shape, API, service workflow, UI, tests, and operational evidence where applicable.
- It should not be a horizontal slice such as "build all database tables" or "create all API routes".
- It should fit in one fresh agent context.
- It should include only the behaviour needed to learn something valuable.
- It should use stubs only where the stub is explicit, isolated, and replaceable.
- It should identify the public seams where TDD will happen.
- It should state what is deliberately out of scope.

Wide refactors are the exception. If one mechanical change fans across the whole codebase, plan it as expand, migrate, contract instead of pretending it is a vertical slice.

## Process

1. Gather context from docs and the current codebase.
2. Name the user-visible behaviour the slice proves.
3. Identify the entry point and end state.
4. Check preconditions and policy gates.
5. Identify the thinnest path through UI, API, service workflow, storage, async boundary, and tests.
6. Identify seams for TDD and whether each seam is already documented or new.
7. Mark dependencies and blockers.
8. Draft the acceptance scenario(s) and get the user's explicit sign-off on the scenario text itself, as its own checkpoint distinct from design-question grilling — see `sdlc-delivery-acceptance-scenario-signoff`. Skip only when the slice has no new user-facing behaviour to verify.
9. Write the slice artifact, embedding the approved scenarios (Test Seams / Acceptance Criteria).
10. Park important deferred work with a revisit trigger instead of leaving it only in Out Of Scope.
11. Ask for approval before implementation if the slice changes scope, policy, or architecture.

## Artifact

For a single slice, write:

`architecture/vertical-slice-0001-<slug>.md`

For several slices, create one file per slice, numbered in dependency order:

`architecture/vertical-slice-0001-<slug>.md`
`architecture/vertical-slice-0002-<slug>.md`

Use this structure:

```markdown
# Vertical Slice 0001: <Name>

## Purpose

The user-visible behaviour this slice proves.

## Source Inputs

- Requirement or scenario references.
- Relevant decisions or ADRs.

## User Path

Given ...
When ...
Then ...

## Preconditions

- Identity and authorization requirements.
- Consent or policy gates.
- Existing records or setup.

## End-To-End Behaviour

Describe the thin path through the system from the user's perspective.

## Layers Touched

- Web UI:
- Core API:
- Analysis Service:
- Storage:
- Queue or async boundary:
- Contracts:
- Observability:

Use "Not touched" where a layer is intentionally excluded.

## Test Seams

- Seam:
- Behaviour verified:
- Test style:

## Data Shape

List only the minimum entities, fields, or contract messages needed for the slice.

## Out Of Scope

- Explicit exclusions.

## Acceptance Criteria

- [ ] Behaviour criterion.
- [ ] Gate or policy criterion.
- [ ] Test or evidence criterion.

## Open Questions

- Questions that must be answered before implementation, if any.
```

## Closeout

At slice or remediation closeout, check whether the work changes the project's traceability chain or AI-SDLC learning record. If it does, update or recommend updating the relevant requirements, product spec, domain model, ADRs, slice docs, acceptance tests, verification report, parking-lot items, or AI-SDLC observations.

## Handoff To Delivery

Before implementation begins, use `sdlc-delivery-acceptance-scenario-signoff` to get the acceptance scenarios themselves reviewed and approved — not just the slice doc's prose summary of them.

When the slice is approved and implementation begins:

- Use `sdlc-delivery-tdd` for red-green work.
- Use `sdlc-delivery-python-service-style` for Python service code.
- Use `sdlc-delivery-typescript-web-style` for React/Vite code.
- Use `sdlc-delivery-dependency-injection` for adapters, storage, queues, model runtimes, and test doubles.
- Use `sdlc-delivery-observability` when adding request IDs, structured events, health/readiness, or cross-service diagnostics.
