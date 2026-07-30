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
- Current persistence: in-memory dev store only, until a persistence decision is made.
- Slice verification command: `pnpm verify:slice`.

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
- Prefer Playwright specs for current UI/browser acceptance until a UI-level Gherkin harness is deliberately introduced.
- Use `pnpm verify:slice` before closing slices or remediation work.
- Keep generated reports honest: they summarize executed checks, not unmeasured coverage or production readiness.

## Closeout

At closeout, check whether the work changes or creates:

- requirements or product specification;
- domain model, glossary, ADRs, or architecture notes;
- vertical slice or remediation docs;
- acceptance tests or verification reports;
- `architecture/parking-lot.md`;
- `requirements/ai-sdlc-observations.md`.

Update the relevant artifact directly when the user asked for implementation or documentation work. Ask first when the change is architectural, policy-sensitive, or changes product intent.
