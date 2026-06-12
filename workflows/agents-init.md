---
category: workflow
tags: [agents, bootstrap, setup, initialization, sync, version-control]
---

# Agents Init

Bootstrap specification for any agent runtime.

## Root Instruction File

Every agent runtime must produce a root instruction file (`AGENTS.md` or equivalent) at its config directory. It must contain:

- Where to find lib-me (and the surrounding knowledge base, if any)
- Reading order for navigating the system
- Which files are authoritative vs. derived
- How to use shared utilities
- How to handle tasks, planning, and work tracking
- The adhoc work management pattern
- When to use execution environments vs. keeping things project-local
- File naming and output conventions

Point at lib-me; do not embed its content. Place this file at the agent's **user-level** config location (e.g. `~/.claude/CLAUDE.md`), not a project-level file — lib-me is personal context that should apply across all sessions and projects.

## Reading Order

1. Root instruction file
2. `lib-me/AGENTS.md` → directory map
3. Relevant `lib-me/stack/` files → environment and tooling
4. `lib-me/workflows/` files → canonical source of skills; set up your runtime's native skills from these files
5. `lib-me/core/` and `lib-me/taste/` → identity and output conventions, when the task calls for them

Load supporting files only when the manifest points to them.

## What Drifts

1. **Pointer layer** — the root instruction file and references to lib-me paths. Picked up automatically on read. No sync needed.
2. **Derived-artifact layer** — skills and instruction files generated from lib-me workflows. Drift when the source changes. Sync required.

## Skill-to-Source Map

| lib-me source | derived artifact |
|---|---|
| `workflows/work-management/` | `work-management` skill |
| `workflows/new-project-setup.md` | `new-project-setup` skill |
| `workflows/agents-init.md` § Adhoc | embedded in each agent's root instruction file |
| `core/*`, `stack/*`, `taste/*`, `README.md`, `AGENTS.md` | none — read live, no regeneration |

Each agent owns the artifacts it generates. A shared directory is written by one owner; never double-write it.

## Version Anchoring

The version anchor is the last commit that touched the lib-me directory:

```bash
git -C <repo-root> log -1 --format=%H -- <lib-me-directory>
```

Not repo HEAD. Each agent stores its last-synced hash in a state file outside the repo.

## Sync Algorithm

1. `CURRENT` = lib-me-scoped HEAD; `LAST` = agent's state file.
2. Equal → up to date, stop.
3. `LAST` missing or hash unreachable → full regeneration.
4. Else: `git diff --name-only $LAST $CURRENT -- <lib-me-directory>/` → changed files.
5. Map changed sources to this agent's artifacts (skip what it doesn't own).
6. Regenerate those artifacts wholesale from current lib-me.
7. Lint; promote on pass, roll back on fail.
8. On promote: write `CURRENT` to state file.

Regeneration runs in a neutral harness that reads only lib-me files and agent registration data — no identity or consumer context loaded. Triggers: on-demand (user command) or scheduled (cron, timer, CI).

## Safety

1. Snapshot current artifacts before running.
2. Lint after generation.
3. Promote on pass; restore snapshot on fail.
4. Never advance the state file for a failed agent.
5. Sync only reads lib-me; only writes agent skills and state files. No commits to the lib-me repo.

## Edge Cases

- **No state file / first run** → full regeneration, then seed the state file.
- **Unreachable hash** (squash/rebase) → full regeneration.
- **Lint failure** → keep old artifacts, do not advance state file.
- **Typo-only diff** → still regenerates wholesale; still advances the pointer.

## Adding a New Agent

1. Create a registration entry: config directory, skill root, what it owns, what it consumes, artifact format.
2. Add the agent's artifacts to the shared ownership map.
3. Run a sync trigger → seeds the state file with current lib-me HEAD.

No changes to the sync wrapper needed; the worker discovers agents from registration entries.

## Adhoc Work Management

The user may maintain a personal adhoc area (e.g. `1-projects/adhoc/` in a knowledge base). Do not move work items elsewhere without explicit instruction.

Adhoc is a container of many mini-projects organized by refinement, not lifecycle:

- `0-wm/` — `todo.md` (flat backlog), `milestones.md` (each `m0x` is one mini-project)
- `1-ideas/` — raw brainstorm; `inbox.md` for one-liners
- `2-plans/` — refined, executable plans

Flow: spark → `1-ideas/` → `2-plans/<name>.md` → milestone + tasks in `todo.md` → execute. Graduation to `1-projects/` is user-triggered only.

## Core Behavior Rules

- Treat lib-me as canonical.
- Do not duplicate durable knowledge in runtime-specific files.
- Keep runtime-native files thin and derived.
- Update lib-me first when a rule changes; then regenerate native files.
- Derive skills from lib-me workflow files, not from runtime memory.
- Do not load `core/profile.md` or the rest of `core/` until explicitly revised and trusted.
- If the runtime has its own instruction hierarchy, map lib-me sections to the nearest native equivalent.
- Keep native files instruction-only; reusable helpers belong in the shared utilities directory.
