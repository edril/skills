---
name: whereami
description: Shows where you are in the current session — big picture by default (overall goal(s)/plan(s), what's done/in-progress/branched-off) or small picture (has recent work drifted from your last request), with an option to save the report to a markdown file
argument-hint: '[small|big|both] (default: big) [save[=path]]'
allowed-tools: Bash(date:*) Glob Read Write
disable-model-invocation: true
---

# Where Am I

Arguments: `$ARGUMENTS` — a mode (`small`, `big`, or `both`; default **`big`** if no mode is given), and optionally `save` or `save=<path>` anywhere in the arguments to also write the report to disk.

This is read-only with respect to the underlying task. Never take further action on the actual work when this is invoked — just report, then hand control back.

## Small picture
Look back to your last message that actually gave direction — an instruction, a decision, an answer to a question you asked — not necessarily your literal last message if that was something else. From there to now, report plainly:

- **What's actually been done** — files touched, commands run, tool calls made, decisions taken
- **Any assumption made without asking** — flag explicitly anywhere the work extended past what was literally asked, even if it seemed reasonable in the moment
- **How it lines up with the request** — matches, has drifted, or scope has crept

End with an explicit fork, and don't resolve it yourself: continue as-is, or pause here and realign before anything else happens.

## Big picture (default)
Reconstruct the session from the start:

- The original goal(s) — if there's been more than one distinct plan/task in this session, keep them separate, don't force one narrative
- What's **complete**, what's **in progress**, what got **branched off and parked** (started, set aside, not yet returned to), what was **abandoned**
- Check the checkpoint directory (default `${CLAUDE_PROJECT_DIR}/docs/session-notes/`, or wherever it's been used in this session) for a file containing `Session ID: ${CLAUDE_SESSION_ID}`. If one exists, read it and ground this report against what's already recorded there — treat it as the anchor, and note anything that's moved on since it was last written. If none exists, reconstruct purely from the live conversation.

Present as a scannable list with a status marker per item (done / in progress / parked / abandoned) — skimmable, not prose.

## `both`
Small picture first, then big picture.

## Saving to a file
If `save` or `save=<path>` appears anywhere in the arguments, also write the report just produced to a markdown file, after showing it in the response as normal.

- Default path: `${CLAUDE_PROJECT_DIR}/docs/session-notes/whereami_<date>_<time>.md`, date/time from `!`date +%Y-%m-%d_%H%M``.
- If `save=<path>` was given explicitly, use that path and just confirm it before writing.
- If only bare `save` was given (no path), state the default path and ask me to confirm it or give a different one — don't write until I respond.
- Write the exact content already shown, with a one-line header noting the mode and timestamp. Confirm the write in one line once done.
