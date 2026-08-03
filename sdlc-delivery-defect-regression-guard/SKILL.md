---
name: sdlc-delivery-defect-regression-guard
description: Guide defect fixes with an explicit regression-test decision. Use when investigating or fixing bugs, production-like defects, QA findings, UI regressions, persistence failures, flaky behaviour, or user-reported broken workflows, especially when deciding whether to write a test before or alongside the fix and what test level should guard against recurrence.
---

# SDLC Delivery Defect Regression Guard

Use this skill whenever a defect is being fixed, unless the user explicitly asks for an exploratory throwaway patch.

The goal is not "always add a big test." The goal is to make every meaningful defect teach the suite one durable lesson at the cheapest useful level.

## Defect Fix Loop

1. **Name the failure.** State the observable failure in product/domain language.
2. **Find the failed promise.** Identify whether the broken promise lives in domain rules, API contract, persistence, UI workflow, visual layout, model/data workflow, operations, or docs.
3. **Choose the regression guard.** Pick the smallest stable test or check that would fail before the fix and pass after it.
4. **Prefer test-first when practical.** Add or adjust the failing test before changing production code when the failure can be reproduced cheaply. If setup would be slow or unclear, write the test immediately after isolating the cause and before final verification.
5. **Fix narrowly.** Change the smallest production surface that satisfies the regression guard without weakening adjacent invariants.
6. **Run focused verification first.** Run the new or changed test plus the smallest relevant command.
7. **Run broader verification when shared seams moved.** Use the project’s standard verification command for changes affecting shared workflows, persistence, API contracts, or browser acceptance.
8. **Record residual debt.** If no test is added, say why. If the defect exposes broader missing architecture, docs, or workflow, update the parking lot or relevant SDLC artifact.

## Guard Selection

- **Domain rule defect:** add or adjust a unit/module/API test at the domain workflow seam.
- **API contract defect:** add route-level request/response tests and, where useful, BDD acceptance coverage.
- **Persistence defect:** add a restart-style test against the durable store. Verify both metadata and dependent artifacts, not only IDs.
- **Object/blob storage defect:** test that stored bytes can be retrieved through the public content endpoint after rebuilding app state or storage adapters.
- **UI workflow defect:** add a Playwright/browser acceptance assertion for the user-visible path.
- **Visual/layout defect:** add a geometry assertion or screenshot-style check only when the failure is likely to recur and can be asserted without brittle pixel perfection.
- **Model/data workflow defect:** add a deterministic fake-adapter test for ordinary CI and, when needed, a separately invoked QA-lane test for the real model/runtime.
- **Tiny cosmetic defect:** a test may be unnecessary if it would be brittle or cost more than the risk. Make that decision explicit.
- **Flaky test:** fix or quarantine with a named reason. Do not silently loosen assertions until the behaviour being protected is clear.

## Quality Bar

A good regression guard:

- fails for the original defect;
- is located at the lowest level that still proves the broken promise;
- asserts observable behaviour or a public seam, not private implementation trivia;
- avoids production/private data unless the user explicitly approves a local recovery or fixture;
- remains deterministic in normal CI unless clearly marked as a local/QA-only check;
- includes live infrastructure only when the defect depends on live infrastructure.

## Closeout

In the final response, include:

- what defect was fixed;
- what regression guard was added or why no guard was added;
- focused checks run;
- broader checks run or why they were not run;
- any residual data repair, manual re-upload, parking-lot, or docs follow-up.
