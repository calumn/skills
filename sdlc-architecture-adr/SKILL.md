---
name: sdlc-architecture-adr
description: Create concise architecture decision records from real trade-offs, especially after requirements, domain modelling, or architecture grilling.
---

# SDLC Architecture ADR

Use this skill when asked to create or update an ADR, architecture decision record, or `architecture/adr/NNNN-*.md`.

## Goal

Record decisions that are hard to reverse, surprising without context, or based on a real trade-off.

Do not create ADRs for obvious, temporary, or low-impact choices.

## Inputs To Inspect

Prefer local artifacts when present:

- `CONTEXT.md`
- `requirements/*.md`
- `architecture/domain-model.md`
- `architecture/domain-model-diagram.md`
- existing `architecture/adr/*.md` or `docs/adr/*.md`

## Location And Numbering

Default to `architecture/adr/`.

Use sequential numbering:

- `0001-short-slug.md`
- `0002-short-slug.md`

Scan the directory for the highest existing number and increment it.

## ADR Format

Prefer concise ADRs:

```md
# Short title

Status: accepted

Context and decision in one to three short paragraphs.

## Consequences

- Important consequence.
- Important trade-off.

## Follow-on Decisions

- Decision branch still open.
- Decision branch that should become a later ADR if it proves important.
```

Include Considered Options only when rejected alternatives are worth remembering.

Include Follow-on Decisions when the ADR deliberately leaves material choices unresolved, such as provider selection, storage technology, queue technology, authentication approach, deployment platform, operational policy, or later service splits. Keep this section short and do not disguise open questions as accepted decisions.

## Review Checks

Before writing, verify:

- The decision is still compatible with the domain model.
- The ADR names the real trade-off.
- The ADR does not hide open questions as settled decisions.
- Material unresolved choices are captured as Follow-on Decisions when useful.
- Any related diagram or system-context doc is linked or updated when useful.
- Important deferred work is added to the project parking lot when it is not suitable for the ADR.

## Closeout

If the decision changes the traceability chain or AI-SDLC learning record, update related requirements, domain model, slice docs, parking-lot items, or AI-SDLC observations when appropriate.
