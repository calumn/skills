---
name: sdlc-delivery-test-automation-reporting
description: Guide automated test harnesses and slice verification reports. Use when adding, reviewing, or repairing Playwright/browser acceptance tests, local test server orchestration, test fixture assets, stable UI test hooks, failure artifacts, repo-level verification commands, or markdown reports summarizing unit, API, BDD, type-check, and browser acceptance checks.
---

# SDLC Delivery Test Automation Reporting

Use this skill when a vertical slice needs executable UI/browser confidence, local developer test commands, CI-ready test harnesses, or a coherent slice-close verification report.

Keep this skill distinct from `sdlc-delivery-acceptance-bdd`:

- Use BDD for executable specification language, Gherkin scenarios, and step mapping.
- Use this skill for the operational machinery that makes tests runnable, stable, observable, and reportable.
- When both apply, define the acceptance behaviour with BDD guidance and implement the browser harness/reporting with this skill.

## Default Workflow

1. Identify the slice behaviour that must be proven from the user's visible path.
2. Choose the highest useful automated seam: unit/module, API, browser, or full slice verification.
3. Add the smallest stable browser acceptance test that proves the implemented user path.
4. Add sparse UI test hooks only where accessible role/name selectors are fragile.
5. Use deterministic fixtures that contain no private user data.
6. Make local execution boring: one command should start required local services or clearly fail with setup guidance.
7. Capture artifacts only when useful, usually on failure.
8. Add or update a slice verification command/report so the user can see what was run before the slice closes.
9. Run the new command and the existing quality loop before reporting success.

## Browser Acceptance Tests

Prefer Playwright for web workflows unless the repo already has a settled browser harness.

Good first browser acceptance tests:

- follow one happy path end to end;
- exercise real UI controls, not API setup shortcuts, unless setup would dominate the test;
- verify user-observable outcomes, caveats, and key visual states;
- assert practical geometry for overlays/canvas/image evidence when layout matters;
- avoid pixel-perfect screenshot baselines until the UI is stable enough to support them.

For Playwright configuration:

- start with one reliable browser, usually Chromium;
- run headlessly by default;
- provide a headed/debug command;
- retain traces, screenshots, and video only on failure unless the user asks for richer artifacts;
- start local dev servers automatically when practical;
- reuse existing servers for local development when safe, but avoid reuse in CI;
- keep base URLs configurable with sensible local defaults.

## UI Test Hooks

Prefer stable user-facing selectors first:

- role and accessible name;
- labels;
- visible text when exact and unambiguous.

Add `data-testid` only when the visible selector would be brittle, ambiguous, or tied to copy that should remain free to evolve.

Good hooks are sparse and domain-shaped:

```text
accept-terms-button
inspection-photo-input
process-analysis-button
evidence-panel
annotation-box
```

Avoid hooks that expose private component structure or layout implementation details.

## Fixtures

Use committed fixtures when visual stability, upload bytes, or debugging clarity matters.

Fixture rules:

- use small files;
- use test-only synthetic content unless the user explicitly provides approved sample data;
- store fixtures near the harness, for example `apps/web/tests/fixtures/`;
- avoid private photos, credentials, personal data, and production exports;
- prefer deterministic assets over generated assets when humans need to inspect failures.

## Slice Verification Reports

Add a report when the user wants a coherent slice-close view of testing evidence.

A useful first report is markdown and includes:

- generated timestamp;
- overall pass/fail;
- each check name;
- exact command run;
- working directory;
- exit code;
- concise summary;
- relevant warning summary;
- failure artifact locations, especially Playwright traces/screenshots/videos;
- a clear note when formal coverage percentages are not measured.

Default report location:

```text
reports/slice-verification/latest.md
```

Recommended command shape:

```text
pnpm verify:slice
```

Keep the report honest. It summarizes checks that ran; it must not imply unmeasured code coverage, production readiness, or manual QA completion.

## Quality Loop

For HiveSight-style service-oriented slices, the report command should usually run:

- Core API tests, including API-level BDD scenarios;
- Analysis Service tests;
- Web TypeScript check;
- Web browser acceptance tests.

Also run lint tools separately when they are not part of the report command.

When browser access is blocked for Codex but runnable by the user or CI, still add the harness and report command. State the limitation clearly and make the command easy for the user to run locally.

## Review Checklist

- Does one command run the browser acceptance path?
- Does one command generate a slice verification report?
- Does the browser test prove behaviour a user can observe?
- Are selectors stable without overfitting to component internals?
- Are fixtures safe, small, and deterministic?
- Are failure artifacts captured without noisy passing-run artifacts?
- Does the report show API-level BDD results when present?
- Does the report avoid false coverage claims?
- Are generated reports and browser artifacts ignored when they should not be committed?
- Are future Gherkin/UI-BDD expectations documented when plain Playwright is used as a temporary first step?
