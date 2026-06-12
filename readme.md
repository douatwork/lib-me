# lib-me

**Why waste time onboarding mythic intellects? They can handle that.**

---

## The problem

Every new AI assistant installs with zero context. You want to try them all, but the re-explaining is such a time sink. Between the new tools, new versions, personal and work machines, and different operating systems, you waste as much time as you save. There's a lot of information about using LLMs more efficiently, but all of it assumes the only people using AI tools are coders. 

## What lib-me is

lib-me teaches agents how to plug in to you rather than you having to plug in to them. The reason LLMs are so effective with coding projects is because the coding project itself provides the foundational context the agent needs to be helpful. When you want to use an LLM as a personal assistant, you have to provide that context. lib-me shows you how to do that. It's an idea more than anything, and there's no special technology involved. It's just files organized into a manual of **you**: your operating context, your tooling, your taste, and your workflows. It's structured in a way that makes sense to humans and enables agentic systems to bootstrap and maintain their own context about you. It is not a framework, a runtime, or a dependency. It's plain Markdown. Point an agent at it and it knows where to look — no integration, no install, no dependencies.

The direction of effort flips. Instead of configuring each new agent, you maintain one manual and every agent plugs into it.

## Quick Start

1. Set up a folder wherever you want and add 4 folders to it: `core`, `stack`, `taste`, and `workflows`.
2. Copy agents-init.md from this repo to the workflows folder.
3. Prompt your agent(s): Read agents-init.md and follow its instructions. The agent will learn about lib-me and self-configure. 

It's that simple. You can fill out as much or as little as you like. Whatever you provide is useful. Whatever you don't provide will not block use. The best part is now that your agent is "read in" to lib-me, it can help you write your manual based on context it already has. From now on, when you're providing important knowledge, start with lib-me or prompt your agent to add it for you.

## Deeper Dive

lib-me is four layers of scaffolding to sort who you are and how you work:

| Directory | What it holds | Answers |
|---|---|---|
| **`core/`** | Identity, beliefs, heuristics | *Who is this person, and why do they decide the way they do?* |
| **`stack/`** | Tools, services, accounts, and resources the human relies on — local environments and hardware through external services and real-world assets | *What does this person have at their disposal, and which of it can I reach from here?* |
| **`taste/`** | Writing voice, code style, naming, sample corpus | *How should the output look and sound when it's done?* |
| **`workflows/`** | Agent bootstrap, project setup, work management | *What's the process for the task in front of me?* |

`core/` and `taste/` are the **"who" and "why."** `stack/` and `workflows/` are the **"what" and "how."** Each directory has its own `readme.md` mapping its contents — start there and expand only as the task calls for it.

The keystone is [`workflows/agents-init.md`](workflows/agents-init.md): the bootstrap spec any runtime follows to find lib-me, navigate it, and keep its derived skills in sync.

## Tips & tricks

Optional ways to make lib-me fit you better — automating agent resync, naming your instance, and where to spend your effort. See [`tips.md`](tips.md).

## Design principles

- **Plain Markdown, no lock-in.** If it can read files, it can use lib-me.
- **Pointer, not payload.** Files point at each other; agents load context lazily instead of swallowing everything up front.
- **One source of truth.** Durable facts live here once; runtime-specific files stay thin and derived.
- **Graceful degradation.** Leave `core/` blank and the rest still works. Fill it in and agents get sharper.
- **Runtime-agnostic.** Today's tools and the one you'll try next month read the same manual.

## License

[MIT](license) — fork it, adapt it, make it yours.
