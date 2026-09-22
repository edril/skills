---
name: sessions
description: Query and analyze the checkpoint corpus in docs/session-notes/ — list, filter, summarize, diff, close, or run free-form pattern analysis across sessions
argument-hint: 'list [status=open|closed|all] [repo=<name>] [pr=<num>] | show <name> | summary <name> | diff <nameA> <nameB> | close <name> | analyze <question>|skills'
allowed-tools: Glob Read Edit
disable-model-invocation: true
---

# Sessions

Arguments: `$ARGUMENTS`

Corpus: `${CLAUDE_PROJECT_DIR}/docs/session-notes/*.md` — the files `/checkpoint` writes. This skill only reads and (for `close`) lightly edits them; it never writes a new checkpoint itself.

## No arguments — quick list
Same as `list status=open`.

## `list [status=open|closed|all] [repo=<name>] [pr=<num>]`
Default `status=open` if not given. Glob the corpus, read each file's frontmatter, filter by whatever was given (`repo=`/`pr=` match inside "Repos & Files Touched"), and list matches: filename, suggested session name, status, repo(s).

## `show <name>`
Fuzzy-match filename or suggested session name. Show the full file content. If more than one matches, list them and ask which.

## `summary <name>`
Match as in `show`. Read the full file and produce a tight synopsis — a few lines covering what it was about, what got done, and how it ended (or, if still open, where it stands) — not a re-print of the file.

## `diff <nameA> <nameB>`
Match each as in `show`. Read both files and report what's meaningfully different: state, decisions, rule packs active, skills invoked, repos touched — not a line-by-line text diff, a substantive comparison of what each session actually covered that the other didn't.

## `close <name>`
Match as in `show`. Edit that file: `Status: open` → `Status: closed`. Confirm briefly.

## `analyze <question>`
Free-form pattern analysis across the corpus, e.g. "which rule packs tend to show up alongside which skills" or "what repos have needed the most checkpoints."

Before answering, ask whether to also pull in the `lessons` knowledge base (`docs/lessons/*.md`) alongside the session notes, unless the question makes it obvious either way.

Read across the matching files and answer the question directly. State plainly that this is a qualitative read across the available files, not a statistical analysis — with the number of sessions likely on hand, patterns spotted this way are worth noting, not treating as proven correlations. If the corpus is too thin to say anything meaningful about the question, say so rather than forcing an answer.

## `analyze skills` — shortcut: which skills are earning their place
1. Glob every skill actually installed — same locations `explain-skills` uses: `.claude/skills/*/SKILL.md`, `.claude/commands/*.md` (project), `~/.claude/skills/*/SKILL.md`, `~/.claude/commands/*.md` (personal).
2. Glob the session corpus and tally how often each one appears in "Skills invoked" across all sessions (open and closed).
3. Report three groups: **frequently used** (clearly earning their place), **rarely used**, and **installed but never appearing in any session** (candidates for review — possibly stale, superseded, or just not yet needed).
4. Same caveat as above: this reflects only sessions checkpointed since the "Skills invoked" field was added, and only what got checkpointed at all — a skill used constantly in sessions that were never checkpointed won't show up. Say this plainly rather than letting a low count look more damning than it is.
