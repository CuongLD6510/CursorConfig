# Commit message — Source Control (SCM)

## What is always on

| Artifact | Path | Effect |
|----------|------|--------|
| Rule | `.cursor/rules/commit-message.mdc` | `alwaysApply: true` — loaded in every Agent/Chat session |
| Skill | `.cursor/skills/commit/SKILL.md` | Auto-discovered when generating commit messages (no `disable-model-invocation`) |
| Command | `.cursor/commands/commit.md` | Explicit `/commit` workflow |

Format: **single line** `[TYPE]: [Message]` with `TYPE` ∈ `feat`, `fix`, `refactor`, `modifier`, `merge`.

## IDE sparkle button limitation

Cursor **Source Control → Generate commit message** uses a separate server flow. As of 2026, **Project Rules are often ignored** for that button; it mainly mimics recent git history.

If the sparkle button still produces wrong format:

1. Copy content from [`templates/cursorrules-commit-snippet.txt`](../templates/cursorrules-commit-snippet.txt) into your **project root** `.cursorrules` (create or merge).
2. Optional: keep it local — `echo .cursorrules >> .git/info/exclude`
3. Stage a small change and test the sparkle button again.
4. Write a few commits manually in the correct format — SCM also learns from `git log`.

## Recommended workflow

- **Reliable format:** `/commit` in Chat, or Agent with staged changes.
- **Hard enforcement:** `commit-msg` git hook (commitlint or custom script) — rejects bad messages at commit time regardless of how they were generated.

## Symlink / copy bundle

When this bundle lives in `~/.cursor/` or a project `.cursor/`, `commit-message.mdc` applies wherever that rules folder is active. The `.cursorrules` snippet is **per repository** and is only needed for SCM sparkle when Rules are not enough.
