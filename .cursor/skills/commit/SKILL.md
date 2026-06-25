---
name: commit
description: Generates git commit messages in format [TYPE]: [Message] (TYPE feat, fix, refactor, modifier, merge) from staged diff. Use when the user invokes /commit, asks for a commit message, uses IDE Source Control Generate commit message, or prepares to commit changes.
---

# Commit

Produce a commit message for the current changes. Do not run `git add` or `git commit` unless the user explicitly asks.

**Always follow** `.cursor/rules/commit-message.mdc` (always applied).

## Steps

1. **Gather state**: Run or reason about `git status` and `git diff --cached` (staged changes only).
2. **Summarize**: One short sentence for what is being committed.
3. **Pick TYPE**: `feat` | `fix` | `refactor` | `modifier` | `merge` (see rule for definitions).
4. **Write message**: Single line `TYPE: Message`.
5. **Output** (see rule):
   - **SCM / Source Control**: plain message line only
   - **Chat / `/commit`**: message in a markdown code block

## Format

```text
feat: Add GET /api/users endpoint and tests
```

## Constraints

- Staged changes only when inferring from git.
- No message if nothing to commit.

## Related

- **Command**: `.cursor/commands/commit.md`
- **Rule**: `.cursor/rules/commit-message.mdc` — always applied; SCM and Chat must follow it
