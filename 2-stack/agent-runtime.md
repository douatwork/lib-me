---
category: stack
tags: [agents, utilities, execution-environments, runtime, tooling]
---

# Agent Runtime Resources

This file defines the shared runtime resources agents may use outside the personal shell and outside project-specific repositories.

## Shared Utilities

Shared agent utilities live in a dedicated utilities directory (e.g., `~/agents/utilities/` or similar).

Use this location for durable command-line helpers that support agent work across runtimes, workflows, or projects.

Rules:

- Prefer an existing utility before creating a new script.
- Keep executable helper scripts in the utilities directory, not inside `.agents/skills`, `.codex`, or other generated instruction folders.
- Generated skills and native instruction files should point to utilities; they should not bundle utility scripts.
- Each utility should have a small `README.md` describing purpose, commands, input/output shape, mutation behavior, and source-of-truth pointers.
- Utilities may read system preferences at runtime, but must not duplicate durable preferences internally.
- Utilities that mutate files, services, databases, or other external state must say so explicitly in their README and command names.
- If a helper becomes generally reusable across tasks, promote it to the utilities directory rather than leaving it inside a project, environment, or generated skill.

## Agent Execution Environments

Repeated adhoc execution environments live in dedicated environment directories (e.g., `~/agents/env*` or similar).

Use these only for repeated adhoc execution needs that arise more than once and need a dedicated runtime or toolchain separate from the personal shell environment.

Rules:

- Do not use execution environments for project-specific setups.
- Project-specific environments belong with the project or repository that owns them.
- Prefer the smallest matching environment that can safely do the job.
- Treat execution environments as disposable infrastructure, not user context and not a source of truth.
- Do not store durable user preferences, project plans, or project knowledge inside execution environments.
- Each environment should have a `README.md` explaining its purpose, activation, installed tools, safe commands, and what not to do there.
- Read the relevant workflow document before entering an environment.
- Switch into an execution environment only when the task requires that runtime.

## Decision Rule

- lib-me / manifests: durable instructions, preferences, and workflow contracts.
- Utilities directory: reusable executable tools.
- Execution environments: repeated adhoc runtimes/toolchains.
- Project or repository folders: project-specific code, execution artifacts, and environments.
- Generated agent-native files: thin instructions that point back to lib-me and shared utilities.

