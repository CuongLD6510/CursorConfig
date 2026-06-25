# Cursor Config Bundle

A reusable **common config** bundle of Cursor commands, rules, skills, and subagents that you can copy or symlink into any project or into your user Cursor directory. When you use this bundle in a project, you can run `/create-config-for-this-project` to generate **project-specific** config (rules, optional commands/skills/subagents) from that codebase.

## Purpose

- **Common config ("Cài đặt chung")**: Commands, rules, skills, and subagents that apply across many projects (e.g. `/task`, `/build`, `/commit`, `/db_diagram`, `/flowchart`, global rules, create-rule/create-skill/create-subagent/migrate-to-skills/update-cursor-settings skills, code-reviewer, planner, and db-diagram subagents). The bundle is **self-contained**: all skills live in `.cursor/skills/`; you do not need to run migrate-to-skills or depend on Cursor's built-in skills. Store them here as a template and optionally copy/symlink to `~/.cursor/` or to each project's `.cursor/`.
- **Project-specific config ("Cài đặt riêng")**: Generated per project by running `/create-config-for-this-project` in that project. This produces default rules (project, sources-base, architecture-patterns) and any extra commands/rules/skills/subagents you request, all under that project's `.cursor/`.

## Repository structure

```
.cursor/
├── commands/     # Slash commands (.md): task, question, build, commit, db_diagram, flowchart, etc.
├── rules/        # Rules (.mdc): global-rules, commit-message (alwaysApply), authoring-guidelines, db-diagram, flowchart
├── skills/       # Skills (each in a subfolder with SKILL.md): task, db-diagram, flowchart, create-user-guide, create-rule, etc.
└── agents/       # Subagents (.md): code-reviewer, planner, flowchart-explorer, db-business-analyst, etc.

docs/
├── COPY-OR-SYMLINK.md     # How to copy or symlink .cursor to ~/.cursor or a project
├── AUTHORING-GUIDELINES.md # Extended guidelines for creating commands, rules, skills, subagents
└── login-monitor/flowchart.md  # Gold-standard example for /flowchart workflow
```

## How to use this bundle

1. **Get the bundle**  
   Clone or download this repo (or copy only the `.cursor/` folder).

2. **Apply the common config**  
   Either:
   - **User-level**: Copy or symlink `.cursor/` (or its subfolders) into `~/.cursor/` so all your projects see the same commands/rules/skills/agents.  
   - **Per-project**: Copy or symlink `.cursor/` into a project's root so only that project uses this bundle.  
   See [docs/COPY-OR-SYMLINK.md](docs/COPY-OR-SYMLINK.md) for step-by-step instructions (Windows PowerShell, macOS, Linux).

3. **Generate project-specific config**  
   Open a project in Cursor, then run:
   ```
   /create-config-for-this-project @current-project
   ```
   Optionally add a brief description of extra config you want, e.g.:
   ```
   /create-config-for-this-project @current-project . Add API response-format rule and convention for DB migrations
   ```
   This creates (or updates) rules and any requested commands/skills/subagents under that project's `.cursor/`.

4. **Use the commands**  
   In any project where this bundle is active, you can use:
   - `/task [brief desc]` — Run a task to completion (Plan or Agents mode).
   - `/question [desc]` — Answer questions only; no file edits.
   - `/build` — Build and auto-fix errors; report status.
   - `/commit` — Commit message `[TYPE]: [Message]`; rule `commit-message.mdc` is always on (Chat + SCM). See `docs/SCM-COMMIT-MESSAGE.md` if the SCM sparkle button ignores format.
   - `/db_diagram [prompt]` — Business/database design docs in `docs/db_diagrams/` (Plan mode recommended).
   - `/flowchart [module-name]` — Business-process Mermaid flowcharts in `docs/{module}/flowchart.md`.
   - `/create-user-guide` — End-user guides (HDSD) from web UI code.
   - `/create-config-for-this-project` — Generate project-specific config.
   - `/create-api-doc-by-controller` — Generate API docs per controller.
   - `/migrate-to-skills` — Convert rules/commands to skills format.
   - `/update-cursor-settings` — Change Cursor user settings (e.g. font, theme, format on save).

## Common vs project-specific

| | Common config | Project-specific config |
|---|----------------|---------------------------|
| **Where** | This repo (template) and/or `~/.cursor/` or a project's `.cursor/` | Generated inside each project's `.cursor/` |
| **Created by** | You (or this repo) | `/create-config-for-this-project` |
| **Content** | Commands, global rules, shared skills/subagents | project-rule, sources-base-rule, architecture-patterns-rule + any requested extras |
| **Reuse** | Same across many projects | Tied to one codebase |

## Documentation

- **[docs/COPY-OR-SYMLINK.md](docs/COPY-OR-SYMLINK.md)** — Copy or symlink `.cursor/` to user or project directory (Windows, macOS, Linux).
- **[docs/AUTHORING-GUIDELINES.md](docs/AUTHORING-GUIDELINES.md)** — How to create and structure commands, rules, skills, and subagents (paths, language, format).
- **[docs/SCM-COMMIT-MESSAGE.md](docs/SCM-COMMIT-MESSAGE.md)** — Commit format for Chat and IDE Source Control; `.cursorrules` workaround for sparkle button.
- **[docs/login-monitor/flowchart.md](docs/login-monitor/flowchart.md)** — Gold-standard Mermaid example for `/flowchart`.

## License and sharing

Use and adapt this bundle for your own or team use. When you copy or symlink into a project, consider committing the project-specific `.cursor/` so the team shares the same config.
