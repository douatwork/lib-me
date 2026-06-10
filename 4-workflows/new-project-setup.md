---
category: workflow
tags: [projects, setup, work-management, planning, execution]
last_updated: 2026-06-05
agent_action: "Read before creating a new project directory and initial workspace structure."
---

# New Project Setup

Use this workflow when starting a new project from scratch.

## Default Location

- Create a new directory named for the project wherever the user indicates it should go.
- Do not invent a different root unless the user asks for one.

## Project Layout

Create these top-level folders inside the project directory:

- `0-wm/`
- `1-init/`
- `2-plan/`
- `3-execute/`
- `4-close/`

## `0-wm/` Setup

Set up the minimal work-management files first:

- `status.md`
- `log.md`
- `todo.md`
- `milestones.md` only if the project needs milestone grouping

Then add `0-wm/README.md` with the relevant work-management instructions for the project.

## `AGENTS.md`

Create `AGENTS.md` in the project root with instructions to:

- read `0-wm/README.md` first
- use `0-wm/` as the task-management system of record
- update `status.md`, `todo.md`, and `log.md` as work changes

## Context Intake

- Read every file in `1-init/` before planning or building.
- Treat `1-init/` as the project context source.

## Output Routing

- Put plans, designs, and ADRs in `2-plan/`
- Put deliverables and code in `3-execute/`
- Put closeout material in `4-close/`

## Working Rule

- Keep the setup minimal by default.
- Expand structure only when the project’s complexity justifies it.
