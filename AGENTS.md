# lib-me — Agent Entry Point

lib-me is a structured manual of a person: their operating context, tooling, taste, and workflows. Read it to bootstrap context about the user without being told. The direction of effort flips — instead of the user configuring each new agent, they maintain one manual and every agent plugs into it.

## Reading Order

1. This file — orientation
2. `stack/README.md` — environment and tooling rules; consult before suggesting commands
3. `workflows/README.md` — task management, project setup, and agent bootstrap
4. `core/README.md` and `taste/README.md` — identity and output conventions, when the task calls for them

Load supporting files only when the relevant manifest points to them.

## Directory Map

| Directory | What it holds | Answers |
|---|---|---|
| `core/` | Identity, beliefs, heuristics | *Who is this person, and why do they decide the way they do?* |
| `stack/` | Tools, services, accounts, environments | *What does this person have at their disposal, and which of it can I reach from here?* |
| `taste/` | Writing voice, code style, naming conventions, sample corpus | *How should output look and sound when it's done?* |
| `workflows/` | Agent bootstrap, project setup, work management | *What's the process for the task in front of me?* |

## Bootstrap Contract

The full specification for initializing from lib-me is in [`workflows/agents-init.md`](workflows/agents-init.md). Read it to understand:

- How to produce your runtime-specific root instruction file
- Which files are authoritative vs. derived artifacts
- How derived skills are versioned and kept in sync
- Behavioral rules every agent follows
