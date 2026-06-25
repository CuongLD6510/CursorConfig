# Flowchart

## Description

Generates or updates business-process flowcharts for a module or screen using Mermaid activity diagrams. Output is user-friendly (no table/API names), horizontal left-to-right layout, with clear start, steps, decision diamonds (Có/Không), and end on every path.

## How to use

```
/flowchart [module-name]
/flowchart [module-name] [screen-or-flow hints]
/flowchart @ControllerOrView.cs
```

- **module-name**: Feature/module slug (e.g. `login-monitor`, `device`, `user`). Used for output path `docs/{module-name}/flowchart.md`.
- **screen-or-flow hints** (optional): Comma-separated screens or flows to include (e.g. `LoginHistory, MailConfig`). If omitted, document all main flows of the module.
- **@file**: Tagged MVC view, controller, or API file — infer module name and flows from code.

## Steps

1. **Load context**: Read and apply:
   - Skill: `.cursor/skills/flowchart/SKILL.md`
   - Rule: `.cursor/rules/flowchart.mdc`
   - Project rules (if present): `project.mdc`, `sources-base.mdc`
2. **Resolve scope**: From arguments or tagged files, determine module name, screens, and background jobs (scheduler, hooks, etc.).
3. **Explore the codebase** (when needed): Use the **flowchart-explorer** subagent (`.cursor/agents/flowchart-explorer.md`) or search controllers, views, models, and docs to list business processes — not implementation details.
4. **Identify flows**: One flowchart per distinct user or system process (e.g. login recording, view history, config save, scheduled report). Merge trivial CRUD into a single “manage list” flow when appropriate.
5. **Write or update** `docs/{module-name}/flowchart.md`:
   - Title: `# {Module Name} — Sơ đồ quy trình nghiệp vụ`
   - Short intro (1–2 sentences)
   - Numbered `## N. {Short title}` per flow, optional one-line description, then Mermaid block
   - Optional **Tham chiếu** links to related docs in the same folder
6. **Validate** against the skill checklist before finishing.

## When to use

- New module or feature needs business flow documentation for BA/QA/operators.
- Existing `flowchart.md` is missing or out of date after UI/API changes.
- User asks for activity diagrams, quy trình nghiệp vụ, or Mermaid flowcharts for a module.

## Examples

**Example 1 — Whole module**  
User: `/flowchart login-monitor`  
Agent: Explores Login Monitor MVC/API, writes `docs/login-monitor/flowchart.md` with flows for login recording, admin access, history view, config, mail, scheduler, etc.

**Example 2 — Specific screens**  
User: `/flowchart login-monitor LoginHistory, MailConfig`  
Agent: Documents only those two areas plus shared “access admin page” if needed.

**Example 3 — From tagged file**  
User: `/flowchart @LoginMonitorController.cs`  
Agent: Infers module `login-monitor`, lists actions/views, generates full module flowchart.

## Important notes

- **Do not** put database table names, API function names, or technical field names in diagram labels.
- **Do** use `flowchart LR` and Vietnamese labels unless the user requests English.
- If `docs/{module-name}/` does not exist, create it.
- Updating an existing file: preserve unrelated sections; refresh only flows that changed unless the user asks for a full rewrite.

## Related

- **Skill**: `.cursor/skills/flowchart/SKILL.md`
- **Rule**: `.cursor/rules/flowchart.mdc`
- **Subagent**: `.cursor/agents/flowchart-explorer.md` — codebase exploration and flow identification
- **Reference example**: `docs/login-monitor/flowchart.md` (gold standard in this bundle)
