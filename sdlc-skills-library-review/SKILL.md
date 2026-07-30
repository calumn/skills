---
name: sdlc-skills-library-review
description: Review and maintain a Codex skills library. Use when auditing skills for stale indexes, broken cross-references, trigger accuracy, project-specific leakage in generic skills, naming drift, missing metadata, duplicated scope, or whether new SDLC skills should be added.
---

# SDLC Skills Library Review

Use this skill to keep a skills library coherent as it grows.

## Inputs

Inspect:

- the skills root directory;
- any `SKILLS_INDEX.md`;
- every relevant `SKILL.md`;
- directly referenced files such as `references/*.md`;
- `agents/openai.yaml` metadata when present.

Do not assume the index is complete. Compare it against the filesystem.

## Checks

Run these checks:

- every skill folder is listed in the index;
- every indexed skill exists;
- skill folder names match `SKILL.md` frontmatter names;
- descriptions include clear trigger language;
- generic skills do not contain project-specific defaults;
- project-specific guidance lives in a project namespace, such as `hivesight-*`;
- cross-referenced skill names resolve;
- referenced files exist;
- stale project names or borrowed-template instructions are removed;
- two skills do not claim the same scope without clear distinction;
- `agents/openai.yaml` metadata matches the skill purpose when present;
- future skill gaps are listed deliberately.

## Findings Format

Lead with findings ordered by severity.

For each finding include:

- affected skill or file;
- problem;
- why it matters;
- recommended fix.

Then include:

- index updates needed;
- skills to add, merge, rename, or delete;
- low-risk mechanical fixes that can be applied immediately;
- decisions the user must make.

## Remediation Rules

- Keep generic skills generic.
- Move reusable project defaults into project-specific skills instead of deleting useful context.
- Keep `SKILL.md` concise; avoid adding auxiliary docs unless progressive disclosure genuinely helps.
- Update the index in the same pass as adding, removing, or materially changing skills.
- Validate changed skills with the skill validator when available.

## Closeout

After remediation, run simple scans for stale names, broken references, and unresolved placeholder markers. Update any project parking lot or AI-SDLC observation log when the skill-library maintenance process changes.
