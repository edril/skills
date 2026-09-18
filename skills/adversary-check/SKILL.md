---
name: adversary-check
description: Self-audits recent output against what was asked, what was agreed, truthfulness/currency, active rules, and general quality — Pass/Concern/Fail per category, with suggested fixes you apply yourself
argument-hint: '[dimension ...] (omit for full checklist): request | agreed | truthful | current | rules | quality'
allowed-tools: Glob Read
disable-model-invocation: true
---

# Adversary Check

Arguments: `$ARGUMENTS` — one or more of the dimensions below, space-separated. No arguments = run all six.

Target: your own most recent substantial piece of output in this session (a response, a file created, a plan) — not a re-check of the whole session. If it's genuinely unclear what "most recent" refers to, ask rather than guessing.

This is read-only. Report only — never apply a fix yourself, even an obvious one, without being separately asked.

## Dimensions

**request** — Does the output actually deliver what was asked, in full? Anything requested but skipped, half-done, or answered adjacent to the real question?

**agreed** — Does it stay consistent with what was specifically discussed and agreed earlier in this session, rather than quietly reverting to a default or a previously-rejected approach?

**truthful** — Any claim stated as fact that wasn't actually verified? Any placeholder, mock, or simulated data presented as if it were real? Anything invented to fill a gap rather than flagged as unknown or assumed?

**current** — Anything presented as current/up-to-date that was actually pulled from training knowledge rather than checked, where it could plausibly have changed?

**rules** — Cross-check against whatever's actually active right now: CLAUDE.md plus any rule pack loaded via `/rules` this session. Same self-audit `/rules recheck` does — if a violation is found, say plainly where.

**quality** — Generic, padded, or template-following output that isn't genuinely tailored to this task — restating the question, filler caveats, boilerplate structure imposed where it didn't fit.

## Output format
One line per dimension checked:

`<dimension> — Pass / Concern / Fail — <one or two lines: what was found, and if not Pass, the suggested fix>`

Close with a quick tally (e.g. "5 Pass, 1 Concern") and stop there — don't act on any of it unless told to.
