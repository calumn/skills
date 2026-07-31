---
name: sdlc-delivery-code-review
description: Review code changes against the project's architecture, domain language, and delivery conventions. Use when reviewing a diff, PR, or pending changes for architecture-to-code alignment, domain vocabulary drift, seam/interface discipline, dead or unauthenticated code paths, and test quality. Distinct from `sdlc-governance-traceability-audit`, which covers doc-to-code drift across the whole SDLC chain rather than a single change.
---

# SDLC Delivery Code Review

Review code changes the way `sdlc-architecture-codebase-design` says code should be built: check depth, seam placement, and domain-language discipline, not just correctness.

## Scope

Review pending changes, a diff, or a PR. Use `sdlc-governance-traceability-audit` instead when the question is whether the whole project's docs and code still agree, not whether one change is sound.

## Inputs

Inspect:

- the diff or changed files, not the whole repository;
- `CONTEXT.md` for domain vocabulary;
- `architecture/codebase-design.md` and any ADRs touching the changed area;
- the vertical-slice or remediation doc the change implements, if any;
- existing tests for the changed seams;
- the project-specific delivery-context skill (for example `hivesight-project-delivery-context`) for known seams and service boundaries.

## Review Passes

Run these in order. Each pass has a different failure mode, and a change can pass one while failing another.

1. **Architecture-to-code alignment.** Does the change match the seam/module shape described in `codebase-design.md` or the relevant ADR? Route handlers, controllers, and components should stay thin — check that behaviour lives in the module the design doc names, not in whichever file was easiest to edit. Flag when a concrete class is depended on where the design doc implies an interface, and when a data-store or client class starts absorbing business rules it wasn't meant to own.
2. **Domain language.** Does naming match `CONTEXT.md`? Flag generic names (`data`, `item`, `handler`) standing in for a defined domain term, and any term used inconsistently with its glossary definition.
3. **Seam and test discipline.** Are new tests written through the module's public interface, or do they reach into internals? Is a new seam introduced only where a second adapter already exists or is clearly imminent, per `sdlc-architecture-codebase-design`'s two-adapter rule?
4. **Correctness and safety.** Bugs, missing error handling, and auth/authorization gaps in the changed code — especially routes that should carry the same auth/scoping as their siblings but don't. Only flag issues in the changed code; pre-existing issues elsewhere belong in `sdlc-governance-traceability-audit`, not here.
5. **Dead or orphaned code.** New code with no caller, or new endpoints/methods whose only caller is their own test. Flag these explicitly rather than letting an unauthenticated or unvalidated stub sit inert until something calls it later.

## Finding Categories

Use the same taxonomy as `sdlc-governance-traceability-audit`, so findings read consistently across skills:

- `Fix now`: active risk, contradiction, or broken promise in the changed code.
- `Document as debt`: real issue, acceptable if visible and tracked — add it to `architecture/parking-lot.md` rather than letting it sit unrecorded in a comment thread.
- `Needs human decision`: architecture, policy, or scope choice the reviewer shouldn't make unilaterally.
- `No issue`: say so explicitly for passes that found nothing. Silence reads as "not checked," not "checked and clean."

Ground every finding in file paths and line numbers.

## Reviewing In Isolation

For anything beyond a small change, dispatch the review as a subagent given only the diff, the requirement or slice it implements, and the inputs above — not the requesting session's history. This keeps the review honest (it evaluates the work product, not the story told about it) and keeps findings, not process, coming back to the main thread.

## Receiving Findings

Verify before implementing. Restate an unclear finding rather than guessing at it. If a finding conflicts with a deliberate prior decision — an ADR, a documented trade-off — say so and resolve the conflict explicitly rather than silently complying or silently ignoring it. Fix `Fix now` items before moving on; carry `Document as debt` items into the parking lot rather than losing them once the conversation ends.

## Closeout

If the review surfaces architecture drift, domain-language drift, or deferred work, update the relevant doc, `architecture/parking-lot.md`, or `requirements/ai-sdlc-observations.md` rather than leaving the finding only in review comments.
