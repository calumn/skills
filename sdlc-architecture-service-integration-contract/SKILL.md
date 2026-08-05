---
name: sdlc-architecture-service-integration-contract
description: Guide the design of an integration contract between two systems you control but build or evolve independently — scoping machine-to-machine auth to a narrow surface, building against a dependency the other side hasn't built yet, and making a cross-system workflow survive suspending on an external human action. Use when scoping a call from one owned app into another, designing a webhook or callback between two of your own services, or deciding how much of an API to expose to a known caller.
---

# Service Integration Contract

Two systems you both own, built and evolved independently, need to talk to each other. This shows up as "app A calls app B's not-yet-built endpoint," "app B needs to notify app A when something finishes, possibly days later," or "how much of our API should the other app actually see." The pattern below is the same regardless of the domain — apiculture, e-commerce, internal tooling — because the problem is structural, not domain-specific.

## Three recurring problems, and the shape of each answer

### 1. Scope the auth to the surface, not just "authenticated or not"

A known-caller integration (you control both ends) rarely needs full user-identity infrastructure. It needs: only this caller, only these routes. Two decisions, kept separate:

- **Mechanism**: for a small number of known callers, a static shared secret (an API key in a header) is legitimate — it's the same cost/value trade-off as any other "build the lightest defensible thing first" decision. Reach for OAuth2 client-credentials (short-lived tokens, scopes, expiry) only once there's a real second caller, or a real need for expiry/rotation. Don't build the heavier mechanism speculatively.
- **Scope**: however the mechanism looks, put the integration's routes under their own prefix/router with their own auth dependency — never reuse the end-user auth dependency for a machine caller, even if it happens to also check a header. Holding the service credential should structurally only reach that router's routes; a static secret has no claims to scope by, so the scoping has to live in the wiring, not the credential.

Write down explicitly which routes the credential does *not* reach — that's usually the thing a quick implementation gets wrong by default (one dependency, reused everywhere it's convenient).

### 2. Build your side against a stub for the dependency that doesn't exist yet

When the other system's endpoint isn't built yet, don't block on it. Define the interface you need as a protocol/interface with exactly one stub implementation now and one real implementation later — the same seam discipline used for any other external dependency (a payment provider, an embedding API, a third-party service). See `sdlc-delivery-dependency-injection` for the general adapter pattern this is an instance of.

This lets you prove your own side's logic completely, and reduces the eventual integration to "swap one adapter," not "build the feature for the first time once the dependency exists."

### 3. If the workflow must wait on an external human action, make the suspend real

Some cross-system workflows can't complete in one request — they hand off, then wait for a human to do something in the *other* system, possibly hours or weeks later (approve, complete, confirm). Two ways this goes wrong:

- Treating the wait as a synchronous block (holds a request/thread open indefinitely — doesn't survive a restart, doesn't scale).
- Building a "resumable" workflow but backing its suspend state with memory instead of durable storage — it looks correct in a quick demo and silently can't survive the exact real-world gap (a process restart, a deploy, a genuinely multi-day wait) it exists to handle.

If you reach for an orchestration tool with checkpointing (e.g. LangGraph) specifically for this reason, verify the checkpoint is backed by real persistence (a database, not the default in-memory store) before treating the suspend/resume behaviour as proven. A test that never actually exercises a process boundary hasn't tested the durability claim, only the code path.

## What to write down

Once these three are decided for a specific integration, they belong in a decision-log-style record (see `sdlc-architecture-adr`) — the reasoning tends to look identical across integrations, so it's worth being able to point back to a prior decision rather than re-deriving it. If the integration spans two separate repos or two separate coding assistants working on each side, the *concrete* contract (endpoint paths, header names, payload shapes, current build status per side) belongs somewhere both sides can see it — a shared, living reference, not a decision buried in only one repo's docs. A project-specific skill naming the concrete contract is one good place for this; a decision log entry in only one of the two repos is not, since the other side's assistant will never load it.

## Relationship to other skills

- `sdlc-delivery-dependency-injection` — the general adapter/seam pattern problem 2 is an instance of.
- `sdlc-architecture-persistence-design` — use when the durable-checkpoint storage in problem 3 needs its own schema/retention thinking.
- `sdlc-architecture-adr` — record the resolved shape of all three decisions once settled.
- `sdlc-delivery-vertical-slice-planning` — this pattern typically surfaces mid-scoping, as one part of a larger slice, not as a slice on its own.
