---
name: sdlc-delivery-python-service-style
description: Guide Python FastAPI service implementation. Use when adding or reviewing Python service code, FastAPI routes, Pydantic schemas, settings, tests, async code, errors, or service-internal modules in a service-oriented codebase.
---

# SDLC Delivery Python Service Style

Use this skill when implementing or reviewing Python service code. Apply it with `sdlc-architecture-codebase-design` when module seams, adapters, or test surfaces matter.

## Core Rules

- Keep route handlers thin: parse HTTP input, call one deep module/workflow, return response models.
- Put behaviour in modules with small interfaces, not in `main.py` or router files.
- Use typed request/response models at service edges, and domain-oriented dataclasses or Pydantic models inside modules when validation matters.
- Prefer explicit return values over hidden mutation or global state.
- Load settings once through a typed settings object; do not read environment variables throughout the codebase.
- Use Python type hints for public module interfaces and adapter interfaces.
- Use `ruff`, tests, and type-aware code review as the minimum quality loop.

## FastAPI Shape

- Split larger services into routers once a second workflow appears.
- Use `Annotated[..., Depends(...)]` for FastAPI dependencies when sharing request context, auth context, settings, or adapters.
- Keep FastAPI dependency injection at the delivery edge; pass plain Python objects into deep modules.
- Use dependency overrides in tests instead of patching globals.
- Represent expected client errors with explicit exceptions mapped to HTTP responses.
- Keep OpenAPI response models accurate; do not return untyped dictionaries from real endpoints.

## Async Guidance

- Use `async` only when the implementation awaits async I/O.
- Do not block the event loop with CPU-heavy model work, image processing, or synchronous network calls inside async handlers.
- Put long-running work behind queues or worker processes.
- Keep image-analysis runtime work out of the Core API request path.

## Testing

- Test HTTP contract shape at route level.
- Test business behaviour through the deep module interface.
- Use in-memory adapters for owned remote seams such as queues or private services.
- Use local-substitutable dependencies where practical, such as a test database or object-store stand-in.
- Do not test private helpers unless they are genuinely complex pure functions.

## BeehiveMonitor Defaults

- Core API route handlers should delegate to modules such as `InspectionPhotoAccess` and `AnalysisRequestWorkflow`.
- Analysis Service route/worker handlers should delegate to `AnalysisJobRunner`.
- Model execution should sit behind `ModelRuntime`.
- Object storage, queue publishing, model runtime, and persistence should be adapters behind module interfaces.

