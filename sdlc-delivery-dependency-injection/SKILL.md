---
name: sdlc-delivery-dependency-injection
description: Guide dependency injection for service seams and adapters. Use when wiring modules, FastAPI dependencies, test doubles, adapters, settings, clients, queues, storage, model runtimes, or owned remote service calls.
---

# SDLC Delivery Dependency Injection

Use dependency injection to make seams explicit, testable, and boring. Apply this with `sdlc-architecture-codebase-design`: dependency injection serves deep modules; it should not create extra layers by itself.

## Core Rules

- Accept dependencies; do not create concrete clients inside business workflows.
- Inject adapters at seams where behaviour varies between production and tests.
- Use FastAPI `Depends` at the HTTP edge, then pass plain Python objects into modules.
- In TypeScript, pass clients/adapters into hooks or workflow modules instead of importing concrete globals everywhere.
- Keep constructors and factories simple; avoid hidden service locators.
- Do not introduce an interface until there are two real adapters or a clear near-term second adapter.

## Good Injection Targets

- settings/configuration
- authenticated user or Workspace context
- repositories/data stores
- object-storage access
- queue publishers/consumers
- Core API clients in the web app
- model runtime adapters
- clock/ID generation when deterministic tests need them
- logger/telemetry interfaces when workflow observability is tested

## Bad Injection Smells

- every helper function gets wrapped in an interface
- route handlers manually assemble large dependency graphs
- tests patch module globals instead of using overrides/adapters
- dependency containers hide what a module actually needs
- production-only dependencies appear in deep module constructors
- UI components import concrete network clients directly

## FastAPI Guidance

- Use dependencies for request-scoped values: settings, auth context, Workspace context, service modules, and adapters.
- Use dependency overrides in tests.
- Keep dependency aliases with `Annotated` when reused across routes.
- Avoid putting business rules inside dependency functions unless the dependency is the intended policy seam, such as `require_workspace_access`.

## Closeout

If dependency changes alter module seams, contracts, tests, domain language, or deferred work, update the relevant architecture docs, ADRs, parking-lot items, or AI-SDLC observations when appropriate.
