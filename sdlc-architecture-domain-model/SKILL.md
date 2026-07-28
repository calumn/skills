---
name: sdlc-architecture-domain-model
description: Derive a domain model and matching visual diagram from requirements, product specs, Gherkin scenarios, model requirements, decision logs, and acceptance criteria before architecture or implementation.
---

# SDLC Architecture Domain Model

Use this skill when asked to create or review a domain model, conceptual data model, entity model, visual domain model, domain vocabulary, or architecture/domain-model.md.

## Goal

Create a technology-neutral domain model that turns requirements into shared vocabulary and stable concepts before implementation.

Always produce or update a visual model alongside the written model.

Do not choose frameworks, databases, or APIs unless the user asks. Capture the domain shape that architecture and implementation must respect.

Use `sdlc-architecture-domain-language` alongside this skill when terminology is still being actively negotiated, a `CONTEXT.md` glossary needs to be updated, or a lightweight ADR may be warranted.

## Inputs To Inspect

Prefer local project artifacts when present:

- vision or problem statement
- requirements document
- product spec or PRD
- Gherkin scenarios or acceptance criteria
- model, data, privacy, or operational requirements
- stakeholder notes
- decision log or ADRs

## Output Structure

Produce two artifacts unless the user explicitly asks for one only:

- `architecture/domain-model.md`: written model.
- `architecture/domain-model-diagram.md`: visual model, preferably Mermaid when the relationships are static.

For the written model, use this structure unless the project already has a stronger convention:

1. Purpose
2. Scope
3. Domain Vocabulary
4. Core Entities
5. Relationships
6. Lifecycle States
7. Key Invariants
8. Derived Values And Calculations
9. Consent, Privacy, And Ownership Boundaries
10. Model And Dataset Governance Concepts, if AI is in scope
11. Traceability To Requirements
12. Open Architecture Questions

For the visual model:

- Use Mermaid `erDiagram`, `classDiagram`, `flowchart`, or `C4Context` style, choosing the simplest diagram that explains the model.
- Keep the first diagram conceptual, not a full database schema.
- Show the main product workflow and any governance/evidence side of the model.
- Include only enough attributes to orient the reader.
- Link the diagram from `domain-model.md`.
- Prefer a separate `domain-model-diagram.md` file when the diagram is large.

## Modelling Rules

- Model user-visible concepts before technical tables.
- Keep product concepts separate from AI/model lifecycle concepts when that improves clarity.
- Include ownership/account boundaries early.
- Capture cardinality in plain language.
- Capture lifecycle states when objects move through review, analysis, consent, approval, or release.
- Capture invariants as rules that must always hold.
- Mark derived values clearly; do not treat calculations as stored facts unless there is a reason.
- Record uncertain or deferred decisions explicitly.
- Keep the written model and visual model consistent.

## AI And Model Systems

When AI output affects the product, include:

- model version
- dataset version
- analysis result
- annotation
- user correction
- review decision
- consent record
- dataset role
- benchmark or release gate concept

Separate model predictions, user corrections, reviewed ground truth, and training/evaluation eligibility.

## Review Checks

Before finalising, check that:

- each core workflow in the spec has supporting entities
- each entity has a clear owner or parent where needed
- key requirements can be traced to entities or relationships
- no major lifecycle state is implicit only in prose
- future out-of-scope concepts are named without overbuilding them
- open questions are specific enough to drive the next architecture decision
