---
name: sdlc-architecture-system-context
description: Create a system context document and visual diagram showing actors, applications, stores, external services, future clients, and trust/security boundaries.
---

# SDLC Architecture System Context

Use this skill when asked to create a system context, C4 context, high-level architecture diagram, or `architecture/system-context.md`.

## Goal

Show how the product sits in its environment before detailed component design.

Keep this technology-aware enough to guide decisions, but not so detailed that it becomes a deployment diagram.

## Inputs To Inspect

Prefer local artifacts when present:

- `CONTEXT.md`
- requirements and product spec
- domain model and diagram
- ADRs

## Output

Produce or update:

- `architecture/system-context.md`
- a Mermaid diagram inside that file, unless the project keeps diagrams separately

## Content To Include

- primary human actors
- main application surface
- backend or application service boundary
- data store
- original file/photo storage
- model or analysis service
- future clients, if relevant
- important trust/privacy boundaries
- open architecture questions

## Trust Boundary Checks

When the system has clients, APIs, storage, internal services, or model/runtime services, make the trust boundaries explicit. Capture:

- which components are public-open, internet-reachable but protected, or private/internal only
- whether each user-facing operation needs user authentication, client/application authentication, Workspace or tenant authorization, or a combination
- where service-to-service credentials are allowed and what they are for
- whether browser or mobile clients must avoid embedded long-lived secrets
- whether upload/download flows use short-lived, object-scoped URLs or direct service streaming
- where rate limiting, request size limits, abuse protection, and gateway/edge checks belong

## Diagram Guidance

Use Mermaid `flowchart` for simple system contexts.

Keep the diagram conceptual:

- actors on the outside
- product boundary in the middle
- storage and external services around it
- label future or deferred pieces clearly

## Review Checks

Before finalising, verify:

- every major domain workflow has a visible system participant
- ownership and data-use boundaries are visible
- public, protected, and private service boundaries are visible
- client authentication, user authentication, authorization, and service-to-service credentials are not blurred together
- storage access patterns do not accidentally expose broad or permanent file access
- future mobile clients are shown as future/deferred, not current scope
- model analysis is clearly separated from model governance where useful
