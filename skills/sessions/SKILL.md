---
name: sessions
description: Query and analyze the checkpoint corpus in docs/session-notes/ — list, filter, summarize, diff, close, or run free-form pattern analysis across sessions
argument-hint: 'list [key=value ...] | show <name> | summary <name> | diff <nameA> <nameB> | close <name> | index | outstanding | analyze <question>|skills'
allowed-tools: Glob Grep Read Edit Write Bash(git:*) Bash(gh:*) Bash(find:*)
disable-model-invocation: true
---

# Sessions

Arguments: `$ARGUMENTS`

Corpus: `${CLAUDE_PROJECT_DIR}/docs/session-notes/*.md` — the files `/checkpoint` writes. This skill never writes a new checkpoint itself. It reads them, edits them only for `close` and `index` (frontmatter only, after confirmation), and writes one derived file, `docs/session-notes/INDEX.md`.

## No arguments — quick list
Same as `list status=open`.

## `list [key=value ...]`
Look sessions up by any recorded metadata. Filters combine (all must match) and values match partially (a SHA prefix, a path fragment):
`status=` · `repo=` · `branch=` · `commit=` · `pr=` · `file=` · `date=` (one day, or a range like `2026-09-01..2026-09-30`, matched against Started..Last active) · `topic=` · `skill=` · `rulepack=` · `issue=` · `resumed-from=`

- With no filters, or with only other filters, search **all** statuses. Default to `status=open` only when no filters at all are given.
- Source: `INDEX.md` if present and not stale (no session file newer than it); otherwise the files' frontmatter, with `file=` matched against "Repos & Files Touched" in the body.
- Show: filename, suggested name, dates, status, repo(s). Keep any `(inferred)` marker visible next to inferred values.

## `show <name>`
Fuzzy-match filename or suggested session name. Show the full file content. If more than one matches, list them and ask which.

## `summary <name>`
Match as in `show`. Read the full file and produce a tight synopsis — a few lines covering what it was about, what got done, and how it ended (or, if still open, where it stands) — not a re-print of the file.

## `diff <nameA> <nameB>`
Match each as in `show`. Read both files and report what's meaningfully different: state, decisions, rule packs active, skills invoked, repos touched — not a line-by-line text diff, a substantive comparison of what each session actually covered that the other didn't.

## `close <name>`
Match as in `show`. Edit that file: `Status: open` → `Status: closed`. Confirm briefly.

## `index` — build or refresh the session library
Goal: one queryable overview of every session, however messy — open, closed, or missing fields. INDEX.md is **derived**: it can always be rebuilt from the files, so there's no process to keep up.

1. Glob `docs/session-notes/*.md`, excluding `INDEX.md`. Read each. If there are many, work in batches and say so. If `INDEX.md` already exists, find what changed with `find docs/session-notes -name '*.md' ! -name INDEX.md -newer docs/session-notes/INDEX.md`. Only those files, plus any file not yet listed in `INDEX.md`, need re-reading and re-proposing; keep the existing rows for everything else. If it doesn't exist yet, index everything.
2. For each session gather: Started / Last active dates, suggested name, Topic, repo(s) with branch / commits / PRs, files touched, Issues raised, rule packs, skills, a one-line summary, what's outstanding (Next Step + Open Questions), and a **judged** status — finished, continued elsewhere, or still live — from content and from whether a later session picked up its Next Step. Don't trust the `Status` field; it may never have been updated.
3. For fields the file already records, use them as-is. For missing ones:
   - **Dates**: from `Started`, the filename, and the last Changelog entry.
   - **Issues raised**: exact — grep `~/.claude/issues/` for the session's `Session ID:`.
   - **Repos / files**: as written in the body; don't add any that aren't there.
   - **Commits / PRs**: only if the file mentions them. Otherwise, for each repo listed that exists locally (the current directory, or a path the file mentions; skip and say so if it can't be found), try recovering by date window: `git -C <repo> log --since=<Started> --until=<Last active + 1 day> --author="$(git config user.name)" --format="%h %ad %s" --date=short`, and best-effort `gh pr list --state all --search <sha> --json number,url` (skip silently if `gh` is unavailable). Mark **every** recovered value `(inferred)` — a date window can pick up unrelated work from the same day.
   - **Rule packs / skills**: `unknown` if not in the file. Never guess.
   - **Topic**: propose one, reusing existing `docs/lessons/` topics where they fit; mark `(inferred)`.
4. Propose links between sessions, each with a one-line reason: `Resumed from` (a later session picks up an earlier one's Next Step), `Duplicates` (substantially the same work), `Related`. These are judgement calls — mark low-confidence ones as such.
5. **Playback before writing.** Show: (a) the proposed index table, (b) the proposed frontmatter additions per original file — only fields that are missing, with `(inferred)` markers, and (c) the proposed links. Let me confirm, edit, or drop each. Write nothing until I respond.
6. On confirmation:
   - Edit each original's **frontmatter only**, adding missing fields in the same shape `/checkpoint` writes (`Last active`, `Topic`, `Repos:` block, `Issues raised`, `Resumed from` / `Duplicates` / `Related`, `Status`, `Rule packs active`, `Skills invoked`). Keep the `(inferred)` marker on anything inferred so it stays distinguishable from recorded fact. Never touch the body.
   - Then write `docs/session-notes/INDEX.md` **last**: one row per session with all of the above. Writing it last means it ends up newer than every file it describes, which is what the change check relies on.

## `outstanding` — what's still open across sessions
1. Read `INDEX.md` if present. Check for staleness with `find docs/session-notes -name '*.md' ! -name INDEX.md -newer docs/session-notes/INDEX.md`; if anything comes back, say the index is stale, name those files, and suggest `/sessions index`. If there's no index, read the session files directly.
2. Group sessions into threads by following `Resumed from` links (a session with no links is its own thread).
3. For each thread, take the **latest** session's Next Step and Open Questions as the outstanding items — earlier sessions in the same thread are treated as superseded.
4. Present a scannable list, most recently active first: thread name, repo(s), last session date, outstanding items. Flag threads that look abandoned (old, no follow-up) and any items that appear to have been resolved in a later session anyway.
5. Judge from content, not from `Status` — show `Status` alongside but don't rely on it.

## `analyze <question>`
Free-form pattern analysis across the corpus, e.g. "which rule packs tend to show up alongside which skills" or "what repos have needed the most checkpoints."

Before answering, ask whether to also pull in the `lessons` knowledge base (`docs/lessons/*.md`) alongside the session notes, unless the question makes it obvious either way.

Read across the matching files and answer the question directly. State plainly that this is a qualitative read across the available files, not a statistical analysis — with the number of sessions likely on hand, patterns spotted this way are worth noting, not treating as proven correlations. If the corpus is too thin to say anything meaningful about the question, say so rather than forcing an answer.

## `analyze skills` — shortcut: which skills are earning their place
1. Glob every skill actually installed — same locations `explain-skills` uses: `.claude/skills/*/SKILL.md`, `.claude/commands/*.md` (project), `~/.claude/skills/*/SKILL.md`, `~/.claude/commands/*.md` (personal).
2. Glob the session corpus and tally how often each one appears in "Skills invoked" across all sessions (open and closed).
3. Report three groups: **frequently used** (clearly earning their place), **rarely used**, and **installed but never appearing in any session** (candidates for review — possibly stale, superseded, or just not yet needed).
4. Same caveat as above: this reflects only sessions checkpointed since the "Skills invoked" field was added, and only what got checkpointed at all — a skill used constantly in sessions that were never checkpointed won't show up. Say this plainly rather than letting a low count look more damning than it is.
