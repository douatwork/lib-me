---
category: workflow
tags: [sync, resync, git, agents, automation, version-control]
last_updated: 2026-06-09
agent_action: "Read when setting up automated resync of an agent's derived skills after lib-me changes. Concept + a prompt; the full contract is in agents-init.md."
---

# Keeping agents in sync with lib-me

When you edit lib-me, one of two things is true:

- **Pointer changes** — you edited `1-core/`, `2-stack/`, `3-taste/`, or `readme.md`.
  Agents read these live, so there's nothing to sync; the next session sees the change.
- **Derived artifacts** — skills or instruction files an agent *generated* from the
  specs in `4-workflows/`. These are real copies in the runtime's native format. They
  drift when the source changes and have to be regenerated.

This tip is about the second case: regenerating an agent's derived skills automatically
when the lib-me source behind them changes — anchored on git commits, no bespoke
tooling required.

## The idea: git commits as the version anchor

lib-me lives in git, so "the version an agent last saw" is just a commit hash. That's
the whole trick:

1. **Anchor on the lib-me directory, not repo HEAD.** The current version is the last
   commit that touched the lib-me directory specifically. If lib-me lives inside a
   larger repo or knowledge base, unrelated edits elsewhere shouldn't trigger a resync.
2. **Each agent remembers its last-synced commit** in a small state file kept *outside*
   the lib-me repo. Missing state, or first run, means a full regeneration.
3. **Detect change by diff.** Compare the last-synced commit to the current one. Equal →
   nothing to do. Different → list the changed source files with a path-scoped diff.
4. **Regenerate only what changed.** Map each changed source to the agent's derived
   artifacts and rebuild those in the runtime's native format. Lint the result.
5. **Promote or roll back.** On a clean lint, replace the old artifacts and advance the
   stored commit. On failure, keep the old version and leave the state file untouched so
   the next run retries.
6. **Never write back to lib-me.** Sync only *reads* the source; it *writes* only the
   agent's own skills and its state file.

The full contract — reading order, the skill-to-source map, and edge cases (a squash or
rebase making the old hash unreachable falls back to full regeneration) — is specified
in [`agents-init.md`](agents-init.md). This doc is just the practical shape of it.

## Triggering it

The mechanism is pure git, so it runs anywhere git runs. *When* it runs is your choice:

- **On demand** — a command you invoke after editing lib-me.
- **Scheduled** — cron, a systemd timer, Task Scheduler, a CI job, an overnight run.
  Use whatever your environment already has.

## Why no script ships here

The detection logic is a handful of git commands; the regeneration is entirely
runtime-specific — every agent has its own skill format and its own config location. A
single shipped script would be either too generic to use or too coupled to one runtime
to share. So instead of code, hand the job to the agent itself: it already knows its own
format. Use the prompt below.

## Prompt: have your agent build its own resync

Copy this to the agent whose skills you want kept in sync. Fill in the two bracketed
values.

```text
Read `[path-to-lib-me]/4-workflows/agents-init.md` — it specifies how this system
versions and syncs derived artifacts. Then set up an automated resync for yourself,
in your own runtime, following that spec:

1. Treat lib-me as the source of truth. The version anchor is the last git commit that
   touched the lib-me directory specifically (NOT repo HEAD).
2. Store your last-synced commit hash in a state file OUTSIDE the lib-me repo, at a
   location appropriate for your runtime. If it is missing, do a full regeneration.
3. On each run, compare your stored commit to the current lib-me-scoped commit. If they
   are equal, stop. If they differ, diff the two over the lib-me directory to find the
   changed source files.
4. Map the changed sources to the skills/instruction files you generate (see the
   skill-to-source map in agents-init.md) and regenerate only those, in your native
   format. Files that are read live — core, stack, taste, readme — need no regeneration.
5. Lint the regenerated artifacts. On success, replace the old ones and advance the
   state file to the current commit. On failure, keep the old artifacts and DO NOT
   advance the state file.
6. Only read from lib-me; only write your own skills and your state file. Never commit
   to the lib-me repo.
7. Give me a command to run this on demand, and tell me how to schedule it with the
   tools in my environment: [cron / systemd / Task Scheduler / CI / other].

Keep it minimal and deterministic. Show me what you create and where you put it before
wiring up any scheduler.
```

The agent reads the contract, builds the part that's specific to it, and you keep your
own trigger. That keeps the shareable thing — the git-anchored mechanism — public, and
leaves the bespoke scheduling where it belongs: with you.
