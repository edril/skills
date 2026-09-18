
---
name: checkpoint
description: Pre-compaction checkpoint — curates decisions, state, and open questions into a resumable markdown file before context fills up
argument-hint: [docs-dir] (optional, defaults to docs/session-notes)
allowed-tools: Bash(date:*) Read Write Edit Glob Grep
disable-model-invocation: true
---

# Session Checkpoint

Target directory: `$ARGUMENTS` if provided, else `${CLAUDE_PROJECT_DIR}/docs/session-notes/`. Create it if missing.

Current session ID: `${CLAUDE_SESSION_ID}`

## Step 1 — Find or start this session's file
Grep the target directory for a markdown file containing the line `Session ID: ${CLAUDE_SESSION_ID}`.

- **Found** → this is an update. Note the filename; merge into it in Step 3.
- **Not found** → this is the first checkpoint of the session.
  - Today's date: !`date +%Y-%m-%d`
  - Build a short slug from the primary task and repo(s) involved
  - Filename: `<date>_<slug>.md`

## Step 2 — Curate, don't dump
This is not a transcript and not an autocompact-style lossy summary. Extract only what's needed to resume cold:
- **Current state** — what's built, mid-flight, or broken
- **Key decisions** — choices made and why, especially ones expensive to re-derive
- **Open questions** — unresolved items, explicitly flagged as pending
- **Repos & files touched** — paths and repo names, not diffs
- **Immediate next step** — specific enough to act on with no other context

Cut anything obvious from re-reading the code, or that doesn't change what happens next.

## Step 3 — Playback before writing
Show the full proposed content:

```
---
Session ID: ${CLAUDE_SESSION_ID}
Started: <date first captured>
Suggested session name: <see Step 5>
---

## Current State
## Key Decisions
## Open Questions
## Repos & Files Touched
## Next Step
## Changelog
- <date/time> — <one-line summary of this checkpoint>
```

If updating an existing file: rewrite "Current State" / "Key Decisions" / "Open Questions" / "Repos & Files Touched" / "Next Step" as **current snapshots** — merge, don't duplicate. Only "Changelog" is append-only: one new dated line per checkpoint.

Ask for confirmation or edits. Do not write until I respond.

## Step 4 — Write
Once confirmed: `Write` for a new file, `Edit` to merge into an existing one.

## Step 5 — Suggest a session rename
Propose a title in the form `<repo(s)>: <task>` (e.g. `cloudtrail-validator, sigma-rules: sigma→kql pipeline`), under ~60 characters. Present it as a suggestion only — there's no confirmed tool for renaming the session itself, so don't attempt it automatically.
