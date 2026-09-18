# lessons

**What it does:** A project-scoped, git-tracked knowledge base of lessons learned — so the same ground doesn't get re-covered every session, and questions get answered from a curated store first instead of an expensive crawl through git history. Not "self-learning" in a literal sense (Claude Code has no background memory pass) — this is the deliberate substitute: cheap capture, cheap recall, curated harvest.

**When to use it:**
- `new` — the moment something's learned (what worked, what didn't, why) — fast, no full review needed
- `recall` — before starting a task, to check whether this ground's been covered, scoped to a topic if you have one in mind
- `harvest` — periodically, to pull candidate lessons out of existing `/checkpoint` and `/issues` files instead of writing everything by hand
- `topics` — to see what scopes already exist before filtering by one

**How to invoke:**
- `/lessons` — overview: topics and counts
- `/lessons topics` — just the topic list
- `/lessons new <topic> <short-name> <lesson text>` — capture one
- `/lessons recall [topic=<topic>] [keyword ...]` — search, scoped by topic first if given; only offers to fall back to a git-log dig if nothing's found in the knowledge base
- `/lessons harvest [topic]` — mines checkpoints/issues for candidates, shows them for confirmation before writing anything

**Where lessons live:** `docs/lessons/*.md` in the project repo itself — git-tracked, commit them like any other file. This is different from `issues` and `rules`, which are personal and live outside any repo; lessons are scoped to "this project" on purpose, per your call.

**Where the skill itself lives:** `~/.claude/skills/lessons/SKILL.md` (personal scope — the skill is yours, the knowledge base it manages is the project's)
