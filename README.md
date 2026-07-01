# draft-goal-prompt Codex Skill

A Codex skill for drafting ready-to-paste Codex `/goal` prompts.

This skill separates a durable long-term `Mission` from the bounded `Current Goal` for the next Codex session. It also adds a `Goal Review`, completion checks, stop boundaries, verification expectations, next-goal candidates, and handoff rules so longer work can continue with less rediscovery.

## Install

When installing from GitHub, point the installer at the skill folder inside this repository.

```bash
CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
python3 "$CODEX_HOME/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo cargo220/draft-goal-prompt-codex-skill \
  --path draft-goal-prompt
```

After installing, start a new Codex session or reload skills, then use `$draft-goal-prompt`.

## Skill Path

```text
draft-goal-prompt/SKILL.md
```

## Notes

- The final `/goal` body should match the user's language. If the user asks in Korean, draft the goal in Korean.
- Nontrivial or high-impact goals include a `Goal Review` before the ready-to-paste `/goal`.
- Responses end with one `Current Goal Summary:` line that summarizes the final `Current Goal`.
- Long-running `Mission` prompts ask Codex to produce `Next Goal candidates`, a `Recommended Next Goal`, and a complete `Ready-to-paste next /goal`.
- The 5-minute unattended-continuation rule is only for safe, in-scope continuation when the user wants unattended progress.
- Silence is never approval for destructive actions, file deletion or moves, installs, downloads, hardware actuation, secret/private data handling, or scope expansion.
