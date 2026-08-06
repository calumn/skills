---
name: hivesight-project-delivery-context
description: Project-specific delivery context for HiveSight. Use when working inside the HiveSight repo and needing local project defaults, domain seams, service boundaries, verification commands, dev auth conventions, or closeout reminders that should not live in generic SDLC skills.
---

# HiveSight Project Delivery Context

Use this skill only for HiveSight-specific delivery context. Pair it with generic `sdlc-*` skills for method guidance.

## Project Facts

- Canonical project name: HiveSight.
- Repo: `hive-sight`.
- Main local path: `/Users/calumnobles/Projects/hive-sight`.
- Service boundaries: Web UI, Core API, Analysis Service.
- Dev auth: Core API requests use `x-hivesight-dev-user-id`.
- Current persistence: dual-mode. Fast workflow/unit tests still use the in-memory dev store; durable Bee Annotation Repository metadata can run through the opt-in Postgres-backed Core API path from Slice 0014.
- Slice verification command: `pnpm verify:slice`.
- Task-oriented user guide: `/Users/calumnobles/Projects/hive-sight/docs/user-guide.md`.
- Local Postgres is provided through Docker Compose and is now required to fully acceptance-close persistence-dependent slices.

## Known Seams

- Web API seam: `CoreApiClient`.
- Photo intake seam: `InspectionPhotoAccess`.
- Analysis request/process seams: `AnalysisRequestWorkflow`, `AnalysisProcessingWorkflow`.
- Dataset seams: dataset labelling, dataset role assignment, Training Crop, Dataset Item, and dataset export workflows.
- Model seams: pre-labeller adapters, model runtime adapters, and Analysis Service job runner.

## Delivery Defaults

- Keep product slices thin, demoable, and testable.
- Preserve stable domain language from `CONTEXT.md`.
- Prefer API-level BDD for service acceptance.
- Slice 0030 introduced the shared acceptance-catalogue pilot: client-neutral Gherkin lives under `acceptance/features/<capability>/...` and can be bound separately through Core API `pytest-bdd` and Web `playwright-bdd` when both seams add confidence.
- Keep plain Playwright specs for browser-only visual, geometry, interaction, and accessibility behaviour. Do not force those details into shared Gherkin.
- For new cross-client behaviours, prefer the shared acceptance-catalogue pattern. For existing slice-history tests, migrate capability-by-capability only when the behaviour is touched or drift risk is high.
- Use `pnpm verify:slice` before closing slices or remediation work.
- When fixing defects in HiveSight, apply `sdlc-delivery-defect-regression-guard`: add the cheapest useful regression guard unless explicitly deciding and reporting why no test was added.
- Treat `pnpm verify:slice` as necessary but not always sufficient. If a slice changes durable persistence, migrations, database constraints, seed/reset behaviour, or restart-survival claims, also verify the live Postgres path or explicitly record why it could not be run.
- Keep generated reports honest: they summarize executed checks, not unmeasured coverage or production readiness.
- Do not mark a Postgres-dependent slice fully acceptance-closed while Docker/Postgres is unavailable. Use wording such as `implemented; fast suite passed; live Postgres verification pending`.

## Local Persistence Commands

Use these from `/Users/calumnobles/Projects/hive-sight` when a slice needs the durable metadata path:

- Start local Postgres: `pnpm db:up`
- Apply migrations: `pnpm db:migrate`
- Reset and seed local database: `pnpm db:reset`
- Start the stack against Postgres: `HIVESIGHT_PERSISTENCE_BACKEND=postgres pnpm dev:all`
- Run the opt-in Postgres persistence test: `HIVESIGHT_TEST_DATABASE_URL=postgresql://hive_sight:hive_sight@localhost:5432/hive_sight_core services/core-api/.venv/bin/python -m pytest services/core-api/tests/test_postgres_persistence_slice.py`

If Docker Desktop is not running or local Postgres cannot be reached, continue with non-database work when useful, but report the persistence verification gap clearly at closeout.

## Closeout

At closeout, check whether the work changes or creates:

- requirements or product specification;
- domain model, glossary, ADRs, or architecture notes;
- vertical slice or remediation docs;
- task-oriented user guide entries in `docs/user-guide.md`;
- acceptance tests or verification reports;
- `architecture/parking-lot.md`;
- `requirements/ai-sdlc-observations.md`.

Update the relevant artifact directly when the user asked for implementation or documentation work. Ask first when the change is architectural, policy-sensitive, or changes product intent.

For persistence-dependent slices, closeout should distinguish:

- implemented;
- verified by fast suite;
- verified by live Postgres;
- verified by browser acceptance;
- blocked or pending because Docker/Postgres was unavailable.
