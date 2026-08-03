---
name: sdlc-delivery-acceptance-scenario-signoff
description: After a vertical slice's design questions are settled (see sdlc-delivery-vertical-slice-planning) but before the slice doc is finalized or implementation begins, draft the actual Gherkin/acceptance scenario text and get the user's explicit sign-off on it as its own checkpoint. Use this whenever a slice will be verified by acceptance scenarios and the user relies on those scenarios — not the code — to trust that the system does what they want, especially when the user has said they don't read code themselves.
---

# Acceptance Scenario Signoff

Design-question grilling settles *how* something will be built. This skill settles a different question: *is this actually what you want to see happen*, asked in the same plain-English scenario text that will later prove the behaviour. For a user who doesn't read code, the Gherkin scenario is the real spec — reviewing it only after implementation, as a side effect of reporting a slice "done," means their one legible checkpoint arrives after the decision it's meant to inform.

## Where this fits

`sdlc-delivery-vertical-slice-planning`'s process runs: gather context → grill design questions → write the slice artifact → implement. Insert this skill between grilling and implementation:

1. Draft mechanism, grill design questions (as that skill already describes).
2. **This skill**: draft the Gherkin scenario(s) that will prove the slice's acceptance criteria. Show the actual feature-file text — `Feature:`, `Scenario:`, `Given/When/Then` — not a prose summary of it. Ask for explicit confirmation or edits, treating the whole draft as one review unit, not a question-by-question grill.
3. Once confirmed, finalize the slice doc (the approved scenario text belongs in its Test Seams / Acceptance Criteria section) and any decision-log entry.
4. Implement via TDD (`sdlc-delivery-tdd`, `sdlc-delivery-acceptance-bdd`) against the scenarios exactly as approved — if implementation reveals the approved wording doesn't quite work, go back to the user with the change and why, don't silently reword it.

Skip this checkpoint only for slices with no new user-facing behaviour to verify (a pure refactor, an internal-only tooling change) — there, note that no new scenario applies and move on, don't force one into existence.

## What "review" actually means here

Don't just paste the draft and wait. Walk the user through what each scenario claims, in plain language, so they can catch:

- **Wrong domain language** — a `Given`/`When`/`Then` using terms the user wouldn't use themselves, or that drifted from the project's glossary/`CONTEXT.md`.
- **Missing scenarios** — a real case the user cares about that isn't covered (an edge case, a failure mode, a second persona).
- **Scenarios nobody asked for** — coverage of something safe-but-irrelevant that adds review burden without adding trust.
- **A `Then` that isn't actually observable to them** — asserting something only visible by reading code or a database row, when the user's whole point of this checkpoint is verifying without doing either.

## Relationship to other skills

- `sdlc-delivery-acceptance-bdd` owns *how* to write good Gherkin (structure, step-definition hygiene, anti-patterns). Use it to actually draft the scenario text this skill asks you to get signed off.
- `sdlc-delivery-vertical-slice-planning` owns the overall slice-doc shape and process; this skill is the extra checkpoint inserted into that process, not a replacement for any of its steps.
- If the project also maintains a requirements-traceability document mapping requirements to scenarios, update it once scenarios are approved and implemented — the signed-off scenario is exactly what that kind of document should point at.

## Why this is worth the extra round-trip

Catching a wrong scenario before code exists to match it costs one conversation turn. Catching it after — once step definitions, fixtures, and application code have all been shaped around the wrong acceptance criteria — costs a rework pass across all of them. For a user whose entire confidence in the system rests on trusting these scenarios without reading anything else, that cost asymmetry is the whole argument for this skill.
