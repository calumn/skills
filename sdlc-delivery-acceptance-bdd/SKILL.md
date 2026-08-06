---
name: sdlc-delivery-acceptance-bdd
description: Guide acceptance tests with BDD, Gherkin, Cucumber-style feature files, pytest-bdd step definitions, executable specifications, and vertical-slice acceptance coverage. Use when writing or reviewing Given/When/Then scenarios, mapping Gherkin to tests, deciding whether behaviour belongs in acceptance tests, or adding Cucumber-like tests to Python services or web workflows.
---

# Acceptance BDD

Use this skill when turning requirements, product specs, acceptance criteria, or vertical slices into executable behaviour examples.

Acceptance BDD is the product-facing test layer. It should explain what a User, Beekeeper, external system, or other actor observes. It should not replace fast module tests, API contract tests, or UI component tests.

## Source Principles

This skill follows the official Cucumber/Gherkin model:

- A `.feature` file is an executable specification.
- A `Scenario` is a concrete example of behaviour.
- `Given` describes initial context.
- `When` describes the action or event.
- `Then` describes an observable outcome.
- Step text is matched to code in step definitions.

For Python services, prefer `pytest-bdd` unless the repo has already chosen another BDD runner. Read [pytest-bdd.md](references/pytest-bdd.md) before adding or changing pytest-bdd step definitions.

## When To Add Gherkin

Add or update Gherkin when:

- a vertical slice needs acceptance coverage;
- the behaviour appears in requirements, product specs, or acceptance criteria;
- the scenario helps non-implementation readers understand what is now true;
- the workflow crosses meaningful product boundaries such as UI → Core API → service workflow;
- permissions, policy gates, or claim boundaries need executable evidence.

Prefer lower-level tests when:

- testing a pure calculation, parser, adapter edge case, or internal state transition;
- enumerating many validation permutations;
- the behaviour is already clearly covered by a broader acceptance scenario;
- the scenario would read like implementation detail rather than product behaviour.

## Scenario Writing Rules

- Use project domain language exactly. Prefer terms defined in the project's glossary or context document.
- Keep scenarios concrete. Avoid vague steps like `Given everything is set up`.
- Keep each scenario centred on one behaviour or business rule.
- Write `Given` steps as preconditions, not user interactions.
- Write one `When` for the main action or event.
- Write `Then` steps as observable outcomes through the chosen acceptance seam.
- Avoid asserting database internals from `Then` steps unless the actor is an internal service and the stored record is the observable contract.
- Keep scenarios short enough to remain readable. If a setup is repeated and important to understanding, consider `Background`; if it is incidental, hide it behind a higher-level step.
- Use `Scenario Outline` only when examples improve clarity more than they add maintenance cost.

## Step Definition Rules

- Bind steps explicitly in code. In pytest-bdd this means `@given`, `@when`, and `@then` decorators whose strings match the feature text.
- Make step text reusable but not generic. Reuse should preserve domain meaning.
- Do not create near-duplicate steps with tiny wording differences. Prefer renaming one step and updating features.
- Keep step functions thin. They arrange state, perform the actor action, or assert an outcome at the selected seam.
- Use shared context fixtures for state passed between steps.
- Avoid global mutable state. Use pytest fixtures, dependency overrides, test clients, or per-scenario context objects.
- Keep detailed edge cases in seam tests; keep Gherkin focused on acceptance examples.

## Choosing The Seam

Pick the highest reliable seam that proves the behaviour:

- **API-level BDD**: default for service-oriented backend slices; fast and stable.
- **Browser-level BDD**: use when the visual/user workflow itself is the behaviour, or when UI state/copy/accessibility is the acceptance target.
- **Service-level BDD**: use when the actor is an internal service or async worker and HTTP/UI would add noise.

For behaviours that matter through more than one client, prefer a single client-neutral feature file with separate binding modules for each seam. The Gherkin should describe domain behaviour only; API steps can perform HTTP calls, browser steps can click controls, and service steps can call workflow objects, but the feature text should not mention routes, buttons, selectors, database tables, or transport details.

Not every scenario needs to run through every seam. Label and document scenarios or feature groups as API-only, browser-only, service-only, or shared when the distinction matters. Keep API contract tests, component tests, visual regression tests, and low-level workflow tests separate from acceptance Gherkin.

When plain browser tests are used instead of browser-level Gherkin, record whether browser Gherkin is deliberately deferred, unnecessary, or parked for a future harness decision.

## Naming And Placement

Default Python service layout:

```text
tests/
  features/
    vertical_slice_0002_analysis_handoff.feature
  test_vertical_slice_0002_bdd.py
```

Use feature filenames that identify the behaviour or slice. Use test filenames that identify the binding layer.

For mature product capabilities, prefer a living capability catalogue over slice-history filenames, for example:

```text
acceptance/
  features/
    payments/
      refund-request.feature
services/api/tests/
  test_refund_request_api_bdd.py
apps/web/tests/bdd/
  steps/
    refund-request.steps.ts
```

Historical vertical-slice documents may keep their signed-off Gherkin as delivery evidence, but the living executable specification should have one canonical home. If a later slice supersedes behaviour, update the canonical feature and binding tests; do not rely on stale slice documents as the current catalogue.

## Review Checklist

- Does the scenario read like product behaviour, not code structure?
- Is the actor clear?
- Is the policy gate or business rule explicit?
- Is the `Then` observable at the chosen seam?
- Are step definitions mapped by explicit decorators?
- Can a failing step point to a useful product regression?
- Are lower-level details covered by faster seam tests instead of bloating the feature?
- Are Gherkin files and step definitions both included in the normal test command?
- If the feature is shared across clients, do all intended bindings execute the same canonical `.feature` file?
- Are API-only, browser-only, service-only, and shared scenarios clearly separated when not every seam should run them?
- Are deferred acceptance-test concerns parked or explicitly closed instead of repeated as vague future work?

## Useful Sources

- Cucumber Gherkin reference: https://cucumber.io/docs/gherkin/reference/
- Cucumber introduction and step definition model: https://cucumber.io/docs/
- pytest-bdd project documentation: https://github.com/pytest-dev/pytest-bdd
