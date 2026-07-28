# Skills Index

This folder uses flat skill directories so Codex can discover skills directly under `/Users/calumnobles/.agents/skills`.

Use prefixes to keep the collection organised without nesting skill folders.

## Naming Convention

- `productivity-*`: general thinking, interview, and working-process skills.
- `sdlc-requirements-*`: requirements gathering, review, specification, Gherkin, and traceability skills.
- `sdlc-architecture-*`: future architecture, domain modelling, ADR, and technical design skills.
- `sdlc-delivery-*`: future implementation planning, testing, release, and production readiness skills.
- `beehive-*`: future BeehiveMonitor-specific skills that should not be treated as general reusable SDLC guidance.

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

## Suggested Future Skills

- `sdlc-requirements-elicit`: guide stakeholder interviews and requirements discovery.
- `sdlc-requirements-write`: turn notes into clear, atomic, testable requirements.
- `sdlc-requirements-trace`: maintain links from goals to requirements, scenarios, tests, and production evidence.
- `sdlc-architecture-adr`: write more formal architecture decision records when a dedicated ADR workflow is needed.
- `sdlc-delivery-test-plan`: turn scenarios and acceptance criteria into a test strategy.
- `beehive-varroa-model-governance`: review Varroa model datasets, annotations, metrics, consent, and release gates.

## Notes

Avoid nesting actual skill directories unless Codex skill discovery is confirmed to support recursive loading. Use this index for human-readable structure instead.
