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
- `productivity-plain-language-git-status`: describe git/version-control actions (commits, branches, merges, PRs) in plain, outcome-focused language instead of jargon.

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
- `sdlc-architecture-persistence-design`: design persistence boundaries, schemas, repositories, migrations, audit/history, and retention.
- `sdlc-architecture-service-integration-contract`: design an integration contract between two systems you own but build independently — scoped machine-to-machine auth, stub-then-real adapters for a dependency the other side hasn't built yet, and durable suspend/resume across an external human action.
- `sdlc-architecture-cross-project-contract-review`: review a design/API proposal arriving from another project touching a shared integration boundary — verify its claims against real code (including empirically, where reading code isn't enough), record the result as a durable response doc, and update the living contract artifact only once implemented. Process companion to `sdlc-architecture-service-integration-contract`.

### SDLC Delivery

- `sdlc-delivery-python-service-style`: guide Python FastAPI service implementation, structure, typing, route handlers, settings, async choices, and tests.
- `sdlc-delivery-typescript-web-style`: guide strict TypeScript React/Vite implementation, API clients, UI state, components, hooks, and tests.
- `sdlc-delivery-observability`: guide logging, structured events, request IDs, traces, metrics, health checks, readiness, and cross-service diagnostics.
- `sdlc-delivery-dependency-injection`: guide dependency injection for service seams, adapters, test doubles, settings, queues, storage, clients, and model runtimes.
- `sdlc-delivery-tdd`: guide red-green test-driven development at documented or agreed seams, with behaviour-focused tests and careful mocking.
- `sdlc-delivery-defect-regression-guard`: guide defect fixes with an explicit regression-test decision at the cheapest useful level.
- `sdlc-delivery-vertical-slice-planning`: define implementation-ready tracer-bullet slices that cut through UI, API, service workflow, storage, tests, and operational evidence where relevant.
- `sdlc-delivery-acceptance-scenario-signoff`: after a slice's design questions are settled but before the slice doc is finalized or implementation begins, draft the actual Gherkin scenario text and get the user's explicit sign-off on it as its own checkpoint — for users who rely on acceptance scenarios, not code, to trust the system.
- `sdlc-delivery-acceptance-bdd`: guide Gherkin, Cucumber-style feature files, pytest-bdd step definitions, client-neutral shared features, seam-specific bindings, and executable acceptance specifications.
- `sdlc-delivery-test-automation-reporting`: guide browser acceptance harnesses, local test orchestration, and slice verification reports.

### SDLC Governance

- `sdlc-governance-traceability-audit`: audit drift across vision, requirements, architecture, slices, code, tests, reports, parking-lot items, and AI-SDLC observations.
- `sdlc-skills-library-review`: review the skills library for stale indexes, broken references, trigger accuracy, project-specific leakage, naming drift, and missing metadata.

### HiveSight

- `hivesight-project-delivery-context`: HiveSight-specific delivery defaults, service boundaries, seams, verification commands, dev auth, and closeout reminders.
- `hivesight-advisor-integration-contract`: the concrete, living cross-app contract between HiveSight and HiveSight Advisor (endpoint status, service-auth header, hive-ID handling) — spans both repos, kept as one shared reference rather than duplicated in each project's own docs.

## Suggested Future Skills

- `sdlc-requirements-elicit`: guide stakeholder interviews and requirements discovery.
- `sdlc-requirements-write`: turn notes into clear, atomic, testable requirements.
- `sdlc-requirements-trace`: maintain links from goals to requirements, scenarios, tests, and production evidence.
- `sdlc-security-threat-model`: threat-model auth, uploads, signed URLs, object storage, privacy, consent, external APIs, and trust boundaries.
- `sdlc-contract-api-governance`: govern REST API, event schema, shared contract, compatibility, versioning, and deprecation changes.
- `sdlc-operations-release-readiness`: guide deployment, release gates, rollout, rollback, smoke tests, runbooks, and incident response.
- `hivesight-varroa-model-governance`: review Varroa model datasets, annotations, metrics, consent, and release gates.

## Notes

Avoid nesting actual skill directories unless Codex skill discovery is confirmed to support recursive loading. Use this index for human-readable structure instead.
