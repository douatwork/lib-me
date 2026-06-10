# lib-me

**Enable your agentic assistants to plug in to you — instead of you plugging in to them.**

---

## The problem

Every new agent starts from zero. Claude Code, Codex, Hermes, OpenClaw, AntiGravity, pi — each one arrives knowing nothing about how you work, what you've already set up, or what "good" looks like to you. So you re-explain. Again. Your conventions, your tools, your voice, your project layout — typed into one more chat box, then lost when the session ends.

The tax compounds the more tools you try. The people most excited about agents pay it most often.

## What lib-me is

lib-me is a portable, agent-readable manual of **you**: your operating context, your tooling, your taste, and your workflows — written once and structured so any agent can read it and set *itself* up.

It is not a framework, a runtime, or a dependency. It's plain Markdown with a predictable shape. Point an agent at it and it knows where to look — no integration, no install.

The direction of effort flips. Instead of configuring each new agent by hand, you maintain one manual and every agent plugs into it.

## How it works

1. **You fill it in** — who you are, what's on your machine, how you like output to look, how you run projects.
2. **You point an agent at it** — one line in the agent's root instruction file (`AGENTS.md`, `CLAUDE.md`, or the runtime's equivalent): *"Read `lib-me/readme.md` first."*
3. **The agent reads the map and self-configures** — it follows the reading order below, loads only what the task needs, and generates its own native skills from the workflow specs.

Change something once and every agent picks it up the next time it reads.

## The map

lib-me is four layers, ordered from "who you are" to "how you work":

| Directory | What it holds | Answers |
|---|---|---|
| **`1-core/`** | Identity, beliefs, heuristics | *Who is this person, and why do they decide the way they do?* |
| **`2-stack/`** | Hardware, environments, tools, runtime resources | *What's available on this machine before I suggest a command?* |
| **`3-taste/`** | Writing voice, code style, naming, sample corpus | *How should the output look and sound when it's done?* |
| **`4-workflows/`** | Agent bootstrap, project setup, work management | *What's the process for the task in front of me?* |

`1-core/` and `3-taste/` are the **"who" and "why."** `2-stack/` and `4-workflows/` are the **"what" and "how."** Each directory has its own `readme.md` mapping its contents — start there and expand only as the task calls for it.

The keystone is [`4-workflows/agents-init.md`](4-workflows/agents-init.md): the bootstrap spec any runtime follows to find lib-me, navigate it, and keep its derived skills in sync.

## Quickstart

```text
1. Fork or copy this repo somewhere your agents can read it.
2. Fill in 1-core/ (you), 2-stack/ (your machine), 3-taste/ (your conventions).
   Every file is a template. Empty files are fine — the system degrades gracefully.
3. Add one line to your agent's root instruction file:
      Read lib-me/readme.md first, then 4-workflows/agents-init.md.
4. Start a session. The agent does the rest.
```

Adopting lib-me with a runtime that doesn't ship support for it? See [`contributing.md`](contributing.md).

## Tips & tricks

Optional ways to make lib-me fit you better — automating agent resync, naming your instance, and where to spend your effort. See [`tips.md`](tips.md).

## Design principles

- **Plain Markdown, no lock-in.** If it can read files, it can use lib-me.
- **Pointer, not payload.** Files point at each other; agents load context lazily instead of swallowing everything up front.
- **One source of truth.** Durable facts live here once; runtime-specific files stay thin and derived.
- **Graceful degradation.** Leave `1-core/` blank and the rest still works. Fill it in and agents get sharper.
- **Runtime-agnostic.** Today's tools and the one you'll try next month read the same manual.

## License

[MIT](license) — fork it, adapt it, make it yours.
