---
name: whereami
description: Shows where you are in the current session — small picture (has recent work drifted from your last request) or big picture (overall goal(s)/plan(s), what's done/in-progress/branched-off), grounded against a /checkpoint file if one exists
argument-hint: '[small|big|both] (default: small)'
allowed-tools: Glob Read
disable-model-invocation: true
---

# Where Am I

Arguments: `$ARGUMENTS` — `small` (default), `big`, or `both`.

This is read-only. Never take further action on the underlying task when this is invoked — just report, then hand control back.

## Small picture (default)
Look back to your last message that actually gave direction — an instruction, a decision, an answer to a question you asked — not necessarily your literal last message if that was something else. From there to now, report plainly:

- **What's actually been done** — files touched, commands run, tool calls made, decisions taken
- **Any assumption made without asking** — flag explicitly anywhere the work extended past what was literally asked, even if it seemed reasonable in the moment
- **How it lines up with the request** — matches, has drifted, or scope has crept

End with an explicit fork, and don't resolve it yourself: continue as-is, or pause here and realign before anything else happens.

## Big picture
Reconstruct the session from the start:

- The original goal(s) — if there's been more than one distinct plan/task in this session, keep them separate, don't force one narrative
- What's **complete**, what's **in progress**, what got **branched off and parked** (started, set aside, not yet returned to), what was **abandoned**
- Check the checkpoint directory (default `${CLAUDE_PROJECT_DIR}/docs/session-notes/`, or wherever it's been used in this session) for a file containing `Session ID: ${CLAUDE_SESSION_ID}`. If one exists, read it and ground this report against what's already recorded there — treat it as the anchor, and note anything that's moved on since it was last written. If none exists, reconstruct purely from the live conversation.

Present as a scannable list with a status marker per item (done / in progress / parked / abandoned) — skimmable, not prose.

## `both`
Small picture first, then big picture.
