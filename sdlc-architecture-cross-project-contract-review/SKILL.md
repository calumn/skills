---
name: sdlc-architecture-cross-project-contract-review
description: Review a design, API proposal, or contract change that arrives from a different project/repo touching a shared integration boundary — verify its claims directly against real code rather than trusting the prose, record the result as a durable response doc, and update the living contract artifact only once implemented. Use whenever another project (or its own AI agent/team) has shared a slice doc, API design, or contract proposal that affects an integration your project sits on the other side of, or when you're the one proposing a change to a contract another project depends on. Also use when deciding whether/how to expose a machine-readable API schema (OpenAPI, JSON Schema, protobuf) at a project boundary.
---

# Cross-Project Contract Review

When two or more projects are each being built (possibly by separate AI coding sessions, possibly by separate teams) and share an integration boundary, changes on one side need review from the other before they're safe to build against. This skill is that review process — a lightweight, repeatable discipline for the "one side proposes, the other verifies and responds" loop, so it doesn't get reinvented ad hoc every time and doesn't quietly lose findings between sessions.

This is the process companion to `sdlc-architecture-service-integration-contract` (which covers the underlying design pattern: scoped auth, stub-then-real adapters, durable suspend/resume). Use that skill to design the integration; use this one to review a proposal that touches it.

## Why this needs to be a deliberate skill, not just "read and reply"

Two things are different here than in an ordinary code review:

- **No memory carries between sessions.** A human staff engineer keeps context in their head across meetings and remembers last quarter's decision without looking it up. An AI session starts cold every time. If a review's outcome isn't written down somewhere durable, it didn't happen as far as any future session is concerned — a chat reply that scrolls away is not a record.
- **The other side's claims deserve verification, not trust — including your own side's claims about itself.** A proposal describing how *your* project's existing API behaves is a claim about your own code, and claims should be checked against that code before being agreed or disputed, not answered from memory of having built it.

## Process

1. **Read the incoming design in full before responding to anything.** Identify what's actually being asked — explicit open questions, review requests — versus what's just background/FYI. Don't answer only the named questions if the surrounding design implies others.

2. **Verify every factual claim about your own side directly against the current code**, not from memory of having built it. This includes: exact request/response shapes, auth mechanisms, error behavior, and anything the incoming design assumes about how your system currently works. If a claim turns out to be inaccurate (the design assumes a shape your API doesn't actually have), say so plainly rather than politely agreeing with something wrong.

3. **For claims that are genuinely uncertain and testable — not just readable from code — verify them empirically.** Some questions can't be answered by reading source (e.g. "does calling this twice in a row behave safely?" often depends on runtime/framework behavior, not just the code's shape). Write a small, throwaway script or test against the real thing and observe what actually happens, the same way you'd verify any other implementation claim. Don't reason from first principles about behavior you could just check.

4. **Sort findings into three kinds, and handle each differently:**
   - **Confirmations** — the proposal is sound as described; say so plainly, don't manufacture disagreement for the sake of having feedback.
   - **Recommendations** — a genuine design choice exists and you have a reasoned preference; state the recommendation and the reasoning, but leave the decision with whoever owns that side.
   - **Bugs or gaps found in your own side while checking** — these are your responsibility, not something to push back across the boundary. Don't fix them silently as a side effect of the review; name them explicitly and treat them as their own follow-up work (see step 6).

5. **Write the response as a durable document in your own repo, not only a chat reply.** One doc per incoming design reviewed, dated, naming what was reviewed and from where. This is what makes the review survive past the current session and be discoverable by the next one — the chat transcript is not a substitute.

6. **If your own side needs to change as a result, scope it as a proper vertical slice** (grilled, tested) rather than patching inline as part of the review. A review's job is to find and record what needs to happen, not to silently implement it under review's cover. Once that follow-up slice actually ships, update the response doc in place (a short "resolved, see Slice NNNN" note at the top) rather than deleting or rewriting the original review — the original record has value as history.

7. **Update the living cross-project contract artifact (the shared integration-contract skill/doc for this pair) only after a proposed change is actually implemented and verified** — never speculatively from a design that hasn't shipped yet. Speculative updates are exactly how that artifact drifts into being a design document instead of a record of what's true, which is the one thing it exists to avoid.

## Contracts as code, not just prose

Once a review's proposal is implemented, the contract it describes should live as a machine-checkable artifact wherever the framework makes that free — not only as prose in a doc. Most modern API frameworks (FastAPI, for example) generate an OpenAPI/JSON-Schema description directly from the same models that validate real requests, at zero extra authoring cost — there's no separate spec to hand-write or let drift out of sync. Where that's available:

- Treat it as the source of truth for the exact shape, over prose descriptions in review docs or skill files (prose is for the *reasoning*, the schema is for the *shape*).
- **The schema is the manifestation of a promise, not just documentation of one.** Once a route is part of a declared external surface (a prefix, a tag, an explicit list — whatever the project uses to mark "other things depend on this"), a breaking change to it is not a routine edit. Enforce that in two places, not one:
  - *At design time* — before implementation, not after — per `sdlc-delivery-vertical-slice-planning`'s rule: a breaking change to declared external surface must be its own explicit grilled decision, with a stated recommendation and sign-off, the same as any other real design fork. Discovering it for the first time in a diff is a process failure, not a successful catch.
  - *At CI time*, as the backstop, not the primary gate — commit/diff the generated schema for the declared-external routes and fail the build on an unannounced breaking change with no accompanying version bump. This exists to catch what the design stage missed, not to replace the design-stage conversation.
- Reserve the *heavier* contract discipline — explicit version fields, deprecation windows, formal breaking-change review — for endpoints that actually cross a project boundary. Applying that ceremony uniformly to every internal, single-consumer endpoint is premature formality; the schema being free doesn't mean the surrounding process should be applied everywhere too.
- Don't assume an endpoint will stay single-consumer forever — "only called by our own UI" is a common trap that quietly stops being true.

## What not to do

- Don't rubber-stamp agreement without checking — a review that only confirms is indistinguishable from one that didn't happen.
- Don't let the review balloon into a full design document of its own; if a genuinely new integration point needs real scoping, that's `sdlc-architecture-service-integration-contract` and `sdlc-delivery-vertical-slice-planning`'s job, not this skill's.
- Don't fix cross-project findings silently inside the review — record them, then scope them properly.
- Don't update the shared living-contract artifact for anything not yet actually built.

## Relationship to other skills

- `sdlc-architecture-service-integration-contract` — the underlying design pattern (scoped auth, stub-then-real adapters, durable suspend/resume) this skill's reviews are usually checking a proposal against.
- `sdlc-delivery-vertical-slice-planning` — scope any follow-up work a review surfaces as its own proper slice, not an inline patch.
- The project-pair's own living contract skill/doc (e.g. an `*-integration-contract` skill) — this is what step 7 updates, and what step 2's verification checks proposals against.
