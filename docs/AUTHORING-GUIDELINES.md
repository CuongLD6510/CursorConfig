# Authoring Guidelines (Extended)

This document is an extended reference for creating and editing **commands**, **rules**, **skills**, and **subagents** in the Cursor config bundle (or when generating project-specific config). For a shorter version applied automatically when editing under `.cursor/`, see the rule `authoring-guidelines.mdc` in `.cursor/rules/`.

---

## 1. File locations and formats

| Artifact  | Path | Format |
|-----------|------|--------|
| **Command** | `.cursor/commands/{command-name}.md` | Plain Markdown, no YAML frontmatter. |
| **Rule**    | `.cursor/rules/{rule-name}.mdc`       | YAML frontmatter + Markdown body. Must be `.mdc` so Cursor recognizes `description`, `globs`, `alwaysApply`. |
| **Skill**   | `.cursor/skills/{skill-name}/SKILL.md`| YAML frontmatter (`name`, `description`) + Markdown body. |
| **Subagent**| `.cursor/agents/{subagent-name}.md`    | YAML frontmatter (`name`, `description`) + body = system prompt. |

- Use **lowercase with hyphens** for file and folder names (e.g. `create-config-for-this-project.md`, `code-reviewer.md`).

### Language policy

- **English**: Meta/authoring artifacts — command structure, `global-rules`, `authoring-guidelines`, create-rule/create-skill/create-subagent skills; skill and subagent `description` frontmatter (third person, with trigger terms).
- **Vietnamese**: Business-workflow content — user-facing responses for `/task`, `/db_diagram`, `/flowchart`, `/create-user-guide`; workflow rule bodies (`db-diagram.mdc`, `flowchart.mdc`); workflow skill instructions and subagent output templates for domain workflows.

---

## 2. Command structure

Each command file (`.cursor/commands/*.md`) should include:

1. **Title** — Single `#` heading (e.g. `# Task`).
2. **Description** — What the command does and when it applies (2–4 sentences).
3. **How to use** — Syntax and arguments, e.g. `/task [brief desc]` and a short explanation of each part.
4. **Steps** — Numbered or bullet list of what the agent must do when the command is invoked.
5. **When to use** — Situations or user intents that match this command.
6. **Examples** — At least one or two concrete examples (user input and expected behavior).
7. **Important notes** — Caveats, dependencies, or references to skills/rules; what the agent must not do.

Additional sections (e.g. **Output format**, **Related commands**) are allowed. Keep the file focused and scannable.

---

## 3. Rule structure (create-rule)

- **Frontmatter** (YAML at top of `.mdc`):
  - `description` (required): Short line for the rule picker — prefer **English** for workflow rules; body may be Vietnamese.
  - `globs` (optional): File pattern, e.g. `**/*.ts`. Omit if the rule applies regardless of file.
  - `alwaysApply` (optional): `true` if the rule should apply in every session when loaded.
- **Body**: Concise, actionable content. One main concern per rule when possible. Prefer under ~50 lines per main topic. Include concrete examples (e.g. good vs bad code or behavior).
- Follow the create-rule skill: total content under 500 lines, actionable, with examples.

---

## 4. Skill structure (create-skill)

- **Frontmatter**:
  - `name`: Lowercase, hyphens, max 64 characters (e.g. `create-rule`, `migrate-to-skills`).
  - `description`: One or two sentences in **third person**, **English**, describing what the skill does and **when** the agent should apply it (trigger scenarios). Vietnamese trigger terms (e.g. HDSD) are allowed in the WHEN clause.
  - `disable-model-invocation`: `true` for slash-command workflows and meta/authoring skills; omit for skills that should auto-apply (e.g. `commit`).
- **Body**:
  - Instructions, workflows, and examples. Keep `SKILL.md` under 500 lines.
  - Use progressive disclosure: put long or detailed content in separate files (e.g. `reference.md`, `examples.md`) and link from `SKILL.md`.
- **References**: Prefer one level deep (e.g. `SKILL.md` → `reference.md`), not long chains.

---

## 5. Subagent structure (create-subagent)

- **Frontmatter**:
  - `name`: Unique identifier, lowercase letters and hyphens.
  - `description`: Third person, **English**; when to delegate. Include trigger terms and "use proactively" if the agent should suggest this subagent automatically.
- **Body**: The system prompt. Define:
  - What the subagent does when invoked.
  - Steps or workflow.
  - Output format and structure.
  - Constraints or guidelines (e.g. "do not edit files", "focus on security").

---

## 6. Cross-references and reuse

- Commands can **reference** skills or rules (e.g. "Follow the migrate-to-skills skill") without duplicating their full content.
- When generating **project-specific** config, reuse these structures and add only project-specific content (paths, naming, conventions). Prefer reusing the same section names (Description, Steps, When to use, Examples, Important notes) for consistency.

---

## 7. Checklist before submitting

**Commands**

- [ ] File is in `.cursor/commands/` with a clear name (e.g. `task.md`).
- [ ] Contains Title, Description, How to use, Steps, When to use, Examples, Important notes.
- [ ] Written in English; examples are concrete.

**Rules**

- [ ] File is `.mdc` in `.cursor/rules/` with valid YAML frontmatter.
- [ ] `description` is set; `globs` / `alwaysApply` only if needed.
- [ ] Body is concise and actionable; includes examples where helpful.

**Skills**

- [ ] Skill lives in `.cursor/skills/{skill-name}/SKILL.md`.
- [ ] Frontmatter has `name` and `description` (third person, with trigger scenarios).
- [ ] Body under 500 lines; long content in linked files.

**Subagents**

- [ ] File is in `.cursor/agents/` with frontmatter `name` and `description`.
- [ ] Description is specific and includes when to delegate (and "use proactively" if desired).
- [ ] Body clearly defines behavior, steps, and output format.
