---
name: sdlc-governance-traceability-audit
description: Audit drift across the full SDLC chain. Use when reviewing whether vision, requirements, product specs, domain models, ADRs, vertical slices, code, tests, verification reports, parking-lot items, and AI-SDLC observations still agree or when closing a tranche of work.
---

# SDLC Governance Traceability Audit

Use this skill to find claims that outran evidence, implemented behaviour that outran documentation, and deferred work that stopped being visible.

## Inputs

Inspect the smallest useful chain for the audit scope:

- vision, goals, or product brief;
- requirements and product specs;
- Gherkin scenarios and acceptance criteria;
- domain model and glossary;
- ADRs and decision logs;
- vertical slice and remediation docs;
- `architecture/parking-lot.md` when present;
- source code and public contracts;
- unit, API, BDD, and browser tests;
- verification reports;
- AI-SDLC observations or learning logs.

## Audit Checks

Look for:

- requirements or vision claims without implemented and tested evidence;
- implemented behaviour not reflected in requirements, slices, or domain model;
- old names, inconsistent terms, or glossary drift;
- duplicate or contradictory artifacts;
- open questions that became load-bearing decisions;
- repeatedly deferred promises with no decision or owner;
- parked items whose revisit trigger has occurred;
- acceptance criteria still unchecked after implementation;
- verification reports that imply more evidence than they actually ran;
- AI/human decision records that stopped being updated.

## Finding Categories

Lead with findings, ordered by severity:

- `Fix now`: active risk, contradiction, broken promise, or misleading artifact.
- `Document as debt`: real issue, but acceptable if visible and tracked.
- `Needs human decision`: product, policy, architecture, or governance choice.
- `No issue`: explicitly say when an inspected area is consistent.

Ground every finding in file paths and line references where possible.

## Remediation Guidance

Prefer updating existing artifacts over creating one more standalone report. If you do create a report, also update the source artifact or parking lot when the issue should persist beyond the current conversation.

When the user asked for implementation or documentation work, update low-risk docs directly. Ask first when the correction changes product intent, architecture direction, security policy, data governance, or scope.

## Closeout

At the end of the audit:

- summarize what is consistent;
- list actioned fixes;
- list remaining parked or remediation items;
- update `architecture/parking-lot.md` for deferred-but-important work when present;
- update AI-SDLC observations when the audit changes how the team works.
