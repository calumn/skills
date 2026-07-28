---
name: sdlc-requirements-review
description: Review requirements, product specs, model requirements, Gherkin scenarios, acceptance criteria, and decision logs for gaps, ambiguity, contradictions, traceability, and readiness for architecture or implementation.
---

# Review Requirements

Use this skill when asked to review requirements or decide whether requirements are ready for the next SDLC phase.

## Goal

Produce a short, decision-oriented requirements review. Do not rewrite everything. Identify what must be fixed now, what can be deferred, and whether the artifacts are ready to move into domain modelling, architecture, design, or implementation.

## Inputs To Inspect

Prefer local project artifacts when present:

- vision or problem statement
- stakeholder notes
- requirements document
- product spec or PRD
- Gherkin scenarios or acceptance criteria
- model, data, security, privacy, or operational requirements
- decision log or ADRs
- traceability notes

## Review Checks

Check for:

- Missing user, actor, or stakeholder coverage.
- Ambiguous language that cannot be tested.
- Requirements that bundle multiple behaviours.
- Conflicts between requirements, spec, scenarios, and decisions.
- Missing acceptance criteria or Gherkin coverage.
- Requirements that imply unapproved scope.
- Hidden architecture or implementation decisions.
- Missing non-functional requirements.
- Missing data, privacy, consent, security, or operational constraints.
- Missing model or AI governance requirements when AI output affects users.
- Traceability gaps between goals, requirements, scenarios, decisions, tests, and evidence.
- Open decisions that block architecture or implementation.

## Output Format

Lead with findings. Group by severity:

- **Blocking**: must resolve before the next phase.
- **Important**: should resolve soon, but work can continue with an explicit assumption.
- **Deferred**: useful later, not needed for the next step.

For each finding include:

- artifact or section
- issue
- why it matters
- recommended action

Then include:

- **Readiness verdict**: ready, ready with assumptions, or not ready.
- **Assumptions**: explicit assumptions that allow progress.
- **Next artifact**: the most useful next document or implementation step.
- **Suggested edits**: concise bullets or patches only when the user asks for changes.

## Review Posture

Be practical. Requirements are never perfect. The question is whether they are good enough to support the next decision without causing avoidable rework.

Avoid broad rewrites, gold-plating, or turning a review into a fresh discovery interview. Ask at most one clarifying question if the review cannot proceed without it.
