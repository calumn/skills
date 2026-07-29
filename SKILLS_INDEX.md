# Skills Index

This folder uses flat skill directories so Codex can discover skills directly under `/Users/calumnobles/.agents/skills`.

Use prefixes to keep the collection organised without nesting skill folders.

## Naming Convention

- `productivity-*`: general thinking, interview, and working-process skills.
- `sdlc-requirements-*`: requirements gathering, review, specification, Gherkin, and traceability skills.
- `sdlc-architecture-*`: future architecture, domain modelling, ADR, and technical design skills.
- `sdlc-delivery-*`: future implementation planning, testing, release, and production readiness skills.
- `hivesight-*`: future HiveSight-specific skills that should not be treated as general reusable SDLC guidance.

## Current Skills

### Productivity

- `productivity-grilling`: one-question-at-a-time stress-test interview for plans, decisions, and ideas.
- `productivity-grill-me`: wrapper skill that starts a grilling session.

### SDLC Requirements

- `sdlc-requirements-to-spec`: synthesize existing conversation and repo context into a product spec with Gherkin scenarios.
- `sdlc-requirements-review`: review requirements artifacts for gaps, ambiguity, contradictions, traceability, and readiness for the next phase.

### SDLC Architecture

- `sdlc-architecture-domain-model`: derive domain entities, relationships, lifecycle states, invariants, open architecture questions, and a matching visual diagram from requirements.
- `sdlc-architecture-domain-language`: sharpen project terminology, maintain a `CONTEXT.md` glossary, and create lightweight ADRs only when warranted.
- `sdlc-architecture-grill-with-docs`: run a grilling session while also maintaining domain language docs and ADRs.
- `sdlc-architecture-adr`: record concise architecture decisions when real trade-offs need to be preserved.
- `sdlc-architecture-system-context`: create high-level system context documents and diagrams.
- `sdlc-architecture-codebase-design`: design deep modules, clean interfaces, seams, adapters, and testable codebase structure.

### SDLC Delivery

- `sdlc-delivery-python-service-style`: guide Python FastAPI service implementation, structure, typing, route handlers, settings, async choices, and tests.
- `sdlc-delivery-typescript-web-style`: guide strict TypeScript React/Vite implementation, API clients, UI state, components, hooks, and tests.
- `sdlc-delivery-observability`: guide logging, structured events, request IDs, traces, metrics, health checks, readiness, and cross-service diagnostics.
- `sdlc-delivery-dependency-injection`: guide dependency injection for service seams, adapters, test doubles, settings, queues, storage, clients, and model runtimes.
- `sdlc-delivery-tdd`: guide red-green test-driven development at documented or agreed seams, with behaviour-focused tests and careful mocking.
- `sdlc-delivery-vertical-slice-planning`: define implementation-ready tracer-bullet slices that cut through UI, API, service workflow, storage, tests, and operational evidence where relevant.

## Suggested Future Skills

- `sdlc-requirements-elicit`: guide stakeholder interviews and requirements discovery.
- `sdlc-requirements-write`: turn notes into clear, atomic, testable requirements.
- `sdlc-requirements-trace`: maintain links from goals to requirements, scenarios, tests, and production evidence.
- `hivesight-varroa-model-governance`: review Varroa model datasets, annotations, metrics, consent, and release gates.

## Notes

Avoid nesting actual skill directories unless Codex skill discovery is confirmed to support recursive loading. Use this index for human-readable structure instead.
