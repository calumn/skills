---
name: sdlc-delivery-typescript-web-style
description: Guide TypeScript React/Vite web implementation. Use when adding or reviewing frontend code, API clients, UI state, strict TypeScript types, React components, hooks, forms, or tests in a web app.
---

# SDLC Delivery TypeScript Web Style

Use this skill when implementing or reviewing TypeScript web code. Apply it with `sdlc-architecture-codebase-design` when client modules, API seams, or test surfaces matter.

## Core Rules

- Keep TypeScript `strict` enabled.
- Avoid `any`; use `unknown` at untrusted boundaries and narrow before use.
- Keep API transport details inside a client module, not inside components.
- Keep components focused on rendering and interaction; move workflow logic into hooks or small modules when it grows.
- Preserve React purity: no side effects during render, no prop/state mutation, and hooks only at the top level.
- Prefer clear domain names from `CONTEXT.md` over generic frontend names.

## API Client Shape

- Use one Core API client seam for the Web App.
- Centralize base URL, auth token attachment, request IDs, response parsing, and error normalization.
- Treat generated OpenAPI types as contract input when available; do not hand-copy response shapes in many places.
- Parse or validate responses at the edge before UI code trusts them.
- Components should call a small client interface, not `fetch` directly, except in throwaway spikes.

## State And UI

- Keep server state, form state, and local interaction state distinct.
- Do not duplicate server state across unrelated components without a deliberate cache strategy.
- Represent loading, ready, empty, and error states explicitly.
- Make async user actions idempotent where possible and show progress.
- Keep accessibility basics in place: labels, button semantics, focus states, and meaningful status text.

## Testing

- Type-check every change.
- Test the API client with mocked fetch or an in-memory adapter.
- Test user workflows at component/page level once workflows become real.
- Avoid tests that assert private component implementation details.

## Closeout

If frontend work changes user-visible scope, API contracts, domain language, acceptance evidence, or deferred work, update the relevant docs, parking-lot items, or AI-SDLC observations when appropriate.
