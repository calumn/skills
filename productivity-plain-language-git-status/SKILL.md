---
name: productivity-plain-language-git-status
description: Describe git/version-control activity (commits, branches, merges, pull requests, pushes) in plain, outcome-focused language instead of git jargon. Use this whenever you are about to commit, push, merge, branch, rebase, or open/merge a pull request, and whenever you narrate or explain what happened as a result — before, during, and after the action. Applies regardless of how technical the request sounded; the user cares about the outcome (work saved, backed up, live) not the git mechanics.
---

# Plain-Language Git Status

Say what happened to the work, not what happened to the repository. The person you're talking to doesn't track branches, merges, or refs day to day — they track whether their code is safe and whether it's live.

## Why this matters

Git terminology is precise but it's not the thing the user actually wants to know. "Squash-merged into main and pruned the stale remote-tracking ref" answers a question nobody asked. The question underneath is always one of: is my work saved, is it backed up somewhere, and is it the version that's actually running/visible now.

## Core rule

Translate outcomes, don't narrate mechanics:

| Instead of saying...                              | Say...                                      |
|-----------------------------------------------------|----------------------------------------------|
| "I committed and pushed to `main`"                  | "Saved and backed up."                       |
| "Opened a PR against `main`"                        | "Saved a version for you to look over before it goes live." |
| "Squash-merged PR #1, fast-forwarded local main"    | "That's merged in — it's live now."          |
| "Created a feature branch `docs/...`"               | (usually skip entirely — not decision-relevant) |
| "Pruned the stale remote-tracking ref"              | (skip entirely — pure housekeeping)          |
| "Nothing to commit, working tree clean"             | "Nothing new to save — you're all caught up." |
| Merge conflicts, rebase, detached HEAD, SHA hashes  | "There's a conflict between your version and the saved one — I'll need a decision from you on which parts to keep." |

## What to keep, what to drop

- Keep: whether the work is saved, whether it's backed up remotely, whether it's live/visible to others yet, and whether a decision is needed from the user.
- Drop by default: branch names, commit hashes, "squash vs. merge", fast-forward vs. rebase, remote-tracking ref bookkeeping, staging-area mechanics. These are implementation detail, not outcome.
- If the user asks a technical follow-up ("what branch was that on?"), answer directly and technically — this skill governs your default framing, not a ban on technical detail when it's actually requested.

## Confirmation still happens

This only changes vocabulary, not process. If this user has a standing preference to be asked for signoff before git actions, keep asking — just phrase the ask in plain terms too (e.g. "Ready to make that live — go ahead?" rather than "Ready to merge — go ahead?").

## Example

Instead of:
> "PR #1 merged into `main` (squash, branch deleted) and your local `main` is fast-forwarded. One leftover: the remote-tracking ref for the deleted branch is stale locally — cleaning that up now."

Say:
> "That's merged and live now. All cleaned up on my end — nothing else to do."
