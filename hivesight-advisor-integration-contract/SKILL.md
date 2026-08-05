---
name: hivesight-advisor-integration-contract
description: The concrete, living cross-application contract between HiveSight and HiveSight Advisor — endpoint paths, the service-auth header, hive-ID handling, and current build status on each side. Use when working in either the hive-sight or hive-sight-advisor repo and reasoning about how the two apps call each other, so both sides' assistants stay aligned without re-deriving or silently renegotiating a contract the other side already committed to.
---

# HiveSight ↔ HiveSight Advisor Integration Contract

HiveSight and HiveSight Advisor are architecturally independent products (confirmed via grilling on the Advisor side, `requirements/decision-log.md`, 2026-07-31), maintained in two separate repos by two separate coding assistants. This skill is the one place both sides can see the actual, current shape of their integration — update it whenever either side's build status or contract changes, rather than letting each repo's own docs drift independently.

For the general reasoning behind the pattern used here (scoped service auth, stub-then-real adapters, durable suspend/resume), see `sdlc-architecture-service-integration-contract`. This skill is the concrete instance of that pattern for this specific pair of apps — it doesn't re-explain the reasoning, only states what was decided.

## Roles

- **HiveSight** owns hive identity, inspection history (photo-based mite counts over time), and treatment history (what was actually applied, when). It is the UI entry point for the integration — a button in HiveSight's own hive-management flow triggers a request into the Advisor.
- **HiveSight Advisor** owns the grounded knowledge/recommendation logic. It is a **read**-dependent consumer of HiveSight's inspection/treatment history, and keeps its own separate record of what it recommended and when — it is never the system of record for what was actually applied to a hive.

## The Contract (current state: 2026-08-05, Slice 0008 implemented and merged)

| Integration point | Direction | Status |
|---|---|---|
| Request a treatment plan for a hive | HiveSight → Advisor | **Built and tested on the Advisor side.** `POST /integrations/hivesight/treatment-plans` — body `{hive_id, jurisdiction_id, situational_context}`, returns `{text, grounding_status, citations}`. Not yet called by a real HiveSight caller. |
| Accept a suggested treatment | Advisor → HiveSight | **Not built on either side yet.** The Advisor calls `TreatmentSuggestionProvider.suggest_treatment(hive_id, answer_text)` — a stub (`StubTreatmentSuggestionProvider`) that records the call but makes no real network request, since HiveSight has no endpoint to receive it yet. |
| Confirm a treatment was completed | HiveSight → Advisor | **Stood in for, on the Advisor side, by a test-only endpoint.** `POST /integrations/hivesight/treatment-plans/completions` — body `{hive_id}`, returns `{id, status}` or 404 if nothing is awaiting completion. Simulates the eventual real HiveSight webhook; HiveSight has not built the real call yet. |

HiveSight's own roadmap (tracked in the `hive-sight` repo, not here) is expected to add: the ability to record treatments against hives, and an endpoint to accept a suggested treatment for a hive. Check there for current status rather than assuming this table is up to date without verifying — this file records what was true as of the date above, not a live feed.

### Auth

Service-to-service calls use a shared-secret header (`X-HiveSight-Service-Key` on the Advisor side), checked via a dedicated FastAPI dependency wired only to the Advisor's `/integrations/hivesight/*` router — never the Beekeeper-facing or Corpus Curator routes. This is deliberately a static shared secret, not OAuth2, per `sdlc-architecture-service-integration-contract` — there is exactly one known caller today. If HiveSight ever needs its own equivalent scoped auth for the reverse direction (Advisor calling into HiveSight), it should follow the same pattern: a dedicated header/dependency scoped to a narrow route prefix, not folded into HiveSight's own user-facing auth.

### Hive identity

HiveSight's hive ID is canonical. The Advisor treats it as an opaque foreign identifier and does not model "Hive" as its own domain entity — this closes what was an open question during Advisor scoping (see the Advisor repo's `requirements/roadmap.md` and `requirements/decision-log.md`, 2026-08-05 entry).

### The suggest → wait → resume shape

The Advisor's workflow for a treatment-plan request is a LangGraph graph: `Recommend` (existing grounded RAG pipeline) → `Suggest` (write to the stub `TreatmentSuggestionProvider`) → `Wait` (suspend, backed by a real Postgres checkpointer — not in-memory, since the wait may span days or weeks) → `Resume` (triggered by HiveSight's eventual completion webhook, closing the Advisor's own recommendation trail). See the Advisor repo's `architecture/vertical-slice-0008-agentic-treatment-plan-request.md` for the full slice detail.

## When this needs updating

Update this file whenever:

- Either side actually builds one of the "not built yet" rows above — change its status and note the real endpoint shape (path, request/response payload).
- The auth mechanism changes (e.g. a move to OAuth2 client-credentials).
- A new integration point is added between the two apps.

Do not let this drift into a design document — if a new integration point needs real scoping/grilling, that belongs in the owning repo's own vertical-slice process; this file only records the settled contract afterward.
