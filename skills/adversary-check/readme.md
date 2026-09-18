# adversary-check

**What it does:** Self-audits your most recent substantial output against six dimensions — does it match what was asked, does it stay consistent with what was agreed, is it truthful/non-simulated, is anything stale presented as current, does it comply with active rules, and is it genuinely good rather than generic filler. Pass / Concern / Fail per dimension, with a suggested fix for anything short of Pass.

**When to use it:** After anything substantial, when you want a structured check rather than trusting the usual after-the-fact adversarial pass happened properly — or any time something feels off and you want it named specifically.

**How to invoke:**
- `/adversary-check` — full six-dimension checklist
- `/adversary-check <dimension> [<dimension> ...]` — just the ones you name: `request`, `agreed`, `truthful`, `current`, `rules`, `quality`

**What it does with problems found:** Reports them with a suggested fix only — it never applies a fix itself. You decide what to act on.

**Where it lives:** `~/.claude/skills/adversary-check/SKILL.md` (personal scope)

**Note on the "built into a rule" idea:** I haven't been able to confirm whether a rule pack's text (loaded via `/rules`) can actually force this skill to run given `disable-model-invocation: true` blocks Claude choosing to invoke it on its own — worth testing directly (add a line like "before finishing, run `/adversary-check`" to a rule pack and see if it actually fires) rather than assuming it works.
