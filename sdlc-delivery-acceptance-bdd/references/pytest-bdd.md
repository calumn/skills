# Pytest-BDD Reference

Read this when adding or changing pytest-bdd tests.

## Minimal Pattern

Feature file:

```gherkin
Feature: Vertical Slice 0002 analysis handoff

  Scenario: Beekeeper processes a queued stub analysis result
    Given the User is logged in with an owner Workspace Membership
    And the Workspace has accepted the Workspace Data Use Agreement
    And the Beekeeper has uploaded an Inspection Photo
    When the queued Analysis Run is processed
    Then the Analysis Run is completed
    And the Beekeeper sees the deterministic stub Analysis Result
```

Step binding file:

```python
from dataclasses import dataclass
from pathlib import Path

import pytest
from fastapi.testclient import TestClient
from pytest_bdd import given, scenarios, then, when

from hive_sight_core_api.main import app


FEATURES_DIR = Path(__file__).parent / "features"

scenarios(str(FEATURES_DIR / "vertical_slice_0002_analysis_handoff.feature"))


@dataclass
class SliceContext:
    client: TestClient
    response_status_code: int | None = None
    response_body: dict[str, object] | None = None


@pytest.fixture
def slice_context() -> SliceContext:
    return SliceContext(client=TestClient(app))


@given("the User is logged in with an owner Workspace Membership")
def user_is_logged_in_with_owner_workspace(slice_context: SliceContext) -> None:
    ...


@when("the queued Analysis Run is processed")
def queued_analysis_run_is_processed(slice_context: SliceContext) -> None:
    ...


@then("the Analysis Run is completed")
def analysis_run_is_completed(slice_context: SliceContext) -> None:
    assert slice_context.response_status_code == 202
```

## Mapping Rules

- `scenarios(...)` links a `.feature` file to the Python module.
- Step decorator strings match the Gherkin step text after the keyword.
- `And` and `But` reuse the preceding step kind, but the matching text still binds to a decorator.
- Put shared per-scenario state in a fixture or context object.
- Use dependency overrides inside fixtures and clear them in `finally` blocks.
- Assertions normally live in `@then` steps.

## Parameterized Steps

Use `pytest_bdd.parsers.parse` when a step needs data:

```python
from pytest_bdd import parsers, then


@then(parsers.parse("the response status is {status_code:d}"))
def response_status_is(slice_context: SliceContext, status_code: int) -> None:
    assert slice_context.response_status_code == status_code
```

Prefer parameterized steps for meaningful examples, not for hiding unclear test logic.

## Anti-Patterns

- Step text copied from route names or function names.
- Step functions that call private helpers or inspect implementation internals.
- Multiple step definitions with nearly identical phrasing.
- Gherkin that enumerates every validation edge case better handled by unit/API tests.
- Feature files that pass only because setup steps assert too much and `Then` asserts too little.
