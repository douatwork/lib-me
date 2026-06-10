---
category: workflow
tags: [skills, workflows, native-format, agent-instruction]
---

# Workflows

These files define reusable workflows and skills. They are the source of truth — agent-specific skill files are derived from them.

## How It Works

Each agent is expected to convert these workflow files into its own native skill format. For example:

- pi uses `SKILL.md` frontmatter with `name` and `description` fields
- Claude Code uses a single markdown file per skill with `name`/`description` frontmatter
- Codex requires both `SKILL.md` and `agents/openai.yaml` per skill

The agent takes the workflow from here, adapts it to its runtime's format, and places it in its skills directory. The workflow file itself is **not** the skill — it's the source.

## Files

- **`agents-init.md`** — bootstrap specification for any agent runtime. Every agent reads this to find the knowledge base and understand the sync system.
- **`new-project-setup.md`** — scaffolding convention for new projects (`0-wm/`, `1-init/`, `2-plan/`, etc.)
- **`work-management/`** — task tracking conventions in two flavors: lite (minimal) and full (complete hierarchy)

## Native Format Generation

Each agent should:

1. Read the workflow file
2. Convert it into its runtime's native skill format (see `agents-init.md` for the skill→source map)
3. Place it in its skills directory
4. The generated skill stays in sync with the source through the versioned sync system

## Future Improvement: Streamlined Sync

Currently, agents regenerate their skills through the full sync algorithm (version anchor → diff → regenerate). A future improvement is a **streamlined per-skill prompt** — instead of regenerating everything on any change, an agent could receive a targeted prompt saying "this workflow changed, regenerate only this skill." This would be lighter weight and faster for small updates.

The current wholesale approach is simple and deterministic; the streamlined approach would be more efficient for high-change scenarios.
