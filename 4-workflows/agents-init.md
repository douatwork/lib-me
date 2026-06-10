---
category: workflow
tags: [agents, bootstrap, setup, initialization, sync, version-control]
---

# Agents Init

This document defines the bootstrap specification for any agent runtime. It describes how an agent finds lib-me, what its root instruction file must contain, how artifacts are versioned and synchronized, and the behavioral rules every agent follows.

Any agentic system — today's tools or something that doesn't exist yet — should produce a root instruction file at its config location following this spec. Nothing else is required.

lib-me is the durable operating manual. It can live as a standalone repo or inside a
personal knowledge base (for example under a PARA-style `2-areas/`). The Knowledge Base
Layout below describes that optional surrounding structure; if you run lib-me standalone,
treat "the knowledge base" as just lib-me itself.

## Bootstrap Contract

Every agent runtime must produce a **root instruction file** (e.g. `AGENTS.md` or its runtime-equivalent) at its configuration directory. This file is the agent's entry point into the system. It contains:

- Where to find lib-me (and the knowledge base, if one surrounds it)
- The reading order for navigating the system
- Which files are authoritative vs. derived
- How to use shared utilities (and what to avoid bundling)
- How to handle tasks, planning, and work tracking
- The adhoc work management pattern
- When to use execution environments vs. keeping things project-local
- Which file naming and output conventions apply

The root instruction file must **point at** lib-me rather than embedding its content. This keeps the file thin and ensures the agent always sees current information.

## Knowledge Base Layout (optional)

When lib-me lives inside a personal knowledge base, that base typically uses a modified PARA structure:

- `0-ingest/` — capture and untriaged material
- `1-projects/` — active outcome-driven work
- `2-areas/` — durable operating context and ongoing responsibilities
- `3-resources/` — useful reference material
- `4-archives/` — inactive or completed material

In that layout, lib-me lives under `2-areas/` as the durable operating manual. Run standalone, lib-me is the manual on its own. Either way, its `readme.md` is the starting point.

## Reading Order

When an agent session begins:

1. Root instruction file → where to start reading
2. `lib-me/readme.md` → directory map and reading rules
3. Relevant `lib-me/2-stack/` files → environment and tooling rules
4. Relevant `lib-me/4-workflows/` files → task management, project setup, agent bootstrap
5. `lib-me/1-core/` and `lib-me/3-taste/` → identity and output conventions, when the task calls for them

Supporting files should only be loaded when the manifest points to them.

## What Drifts (and What Doesn't)

Two layers relate to lib-me; only one needs synchronization:

1. **Pointer layer** — the root instruction file and derived artifacts that reference lib-me paths. Changes to `2-stack/`, `readme.md`, and `3-taste/` are picked up automatically by file reads. **No sync needed** for pointer changes.
2. **Derived-artifact layer** — skills and instruction files generated from lib-me workflows. These are real copies in the runtime's native format and drift when the source workflow changes. **Sync required.**

Embedded content blocks (verbatim copies of lib-me sections) in root instruction files are part of the pointer layer and drift when the source changes.

## Skill-to-Source Map

Skills are derived from lib-me workflow files, not hardcoded into the agent's runtime. The mapping follows this principle:

| lib-me source | derived artifact |
|---|---|
| `4-workflows/work-management/` | `work-management` skill |
| `4-workflows/new-project-setup.md` | `new-project-setup` skill |
| `4-workflows/agents-init.md` § Adhoc | embedded in each agent's root instruction file |
| `1-core/*`, `2-stack/*`, `3-taste/*`, `readme.md` | none — read live, no regeneration |

Each agent owns the artifacts it generates and consumes shared skills written by another agent. A shared directory holds skills consumed by multiple runtimes, written by a single owner.

### Ownership Rule

A location is written **once per sync run, by its owner's regeneration**. Never double-write a shared directory. The owner must emit the union of all consumers' requirements.

## Version Anchoring

lib-me lives in a git repo. The version anchor is the last commit that touched the lib-me directory specifically:

```bash
git -C <repo-root> log -1 --format=%H -- <lib-me-directory>
```

Not repo HEAD (which advances on any edit elsewhere in the repo). Each agent records the lib-me commit it last synced from in a state file outside the repo, at a location determined by its runtime.

## Sync Algorithm

A per-agent sync works like this:

1. `CURRENT` = lib-me-scoped HEAD; `LAST` = this agent's state file.
2. If equal → up to date, skip.
3. If `LAST` missing or the hash is unreachable (history rewritten) → full regeneration of all this agent's artifacts.
4. Else: compute `git diff --name-only $LAST $CURRENT -- <lib-me-directory>/` → changed files.
5. From the agent's registration data, map changed sources to its artifacts (skip locations it does not own).
6. Regenerate those artifacts wholesale from current lib-me, applying the requirement superset.
7. Lint; promote or roll back.
8. On promote, write `CURRENT` to the state file.

## Worker Runtime

Regeneration is performed by an agent runtime acting as a **neutral execution harness**. It reads only the lib-me files and the agent registration data — it must not load its own identity or consumer context. Each agent is treated as pure data from its registration record.

## Triggers

| trigger | who | what |
|---|---|---|
| on-demand command | user | immediate regeneration |
| periodic timer | system | scheduled unattended run |

## Safety

The sync wrapper enforces:

1. Snapshot current artifacts before invoking the worker.
2. Worker runs; lint checks format validity.
3. Promote on pass; restore snapshot on fail.
4. Never advance the state file for a failed agent.

An audit trail is maintained in a log file, not in git.

## Edge Cases

- **First run / no state file** → full regeneration, then seed the state file.
- **Unreachable hash** (after squash/rebase) → fall back to full regeneration.
- **Lint failure** → keep snapshot (old version), do NOT advance state, flag in log.
- **Pure typo / no-semantic-change diff** → still regenerates wholesale (cheap + deterministic) and advances the pointer.

## No Git Writes

Sync only *reads* lib-me; it *writes* only outside the lib-me repo — regenerated artifacts into agent directories and state files. The audit trail lives in a log file.

## Adding a New Agent

1. Create a registration entry for the agent describing its config directory, skill root, what it owns, what it consumes, and the format of its generated artifacts.
2. Add the agent's artifacts to the shared directory ownership map.
3. Run the sync trigger for the first full regeneration — this seeds the state file with the current lib-me HEAD.
4. Periodic triggers pick it up automatically from that point on.

No changes to the sync wrapper or worker invocation are needed — the worker discovers agents by reading the registration entries.

## Adhoc Work Management

The user may maintain a personal adhoc work-management area (for example under a knowledge base's `1-projects/adhoc/`). Agents must NOT decide to move work items elsewhere; use this folder when the user asks for "adhoc" tasks.

Unlike a normal project, adhoc is a **container of many mini-projects**, each at a different stage. Organized by **refinement** (how cooked an idea is), not by lifecycle phase:

- `0-wm/` — work management: `todo.md` (flat backlog), `milestones.md` (always present; each `m0x` is one mini-project linking to its plan).
- `1-ideas/` — raw, pre-commitment brainstorm (one file per spark; `inbox.md` for one-liners).
- `2-plans/` — refined, executable plans (one file per committed mini-project).

Flow: spark → `1-ideas/` → refine into `2-plans/<name>.md` → commit as a `{m0x}` milestone + tasks in `todo.md` → execute.

**Graduation rule:** when a mini-project needs real execution artifacts (its own code, design docs, ongoing maintenance), move it to `1-projects/` (user-triggered only).

Core instruction for root files:

> The user's adhoc work-management area is personal. Do not move adhoc work items to other projects unless the user explicitly asks.

## Core Behavior Rules

- Treat lib-me as canonical.
- Do not duplicate durable knowledge in runtime-specific files if lib-me can hold it.
- Keep runtime-native files thin and derived.
- Update lib-me first when the underlying rule changes; then regenerate native files.
- Derive skills from lib-me workflow files, not from stale runtime memory.
- Do not ingest `1-core/profile.md` or the rest of `1-core/` until explicitly revised and trusted.

## Special Cases

- If the agent runtime has its own instruction hierarchy, map lib-me sections into the nearest native equivalent.
- If a file is only supporting material, do not load it until the relevant manifest points to it.
- Keep native files instruction-only when a reusable helper can live in the shared utilities directory.
