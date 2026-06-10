# Adopting & extending lib-me

lib-me is a personal manual, so the main way to "contribute" is to **make your own**. This guide covers adopting it for yourself and wiring it to an agent runtime that doesn't ship support.

## Make it yours

1. **Fork or copy** this repo to wherever your agents can read it — a standalone repo, or inside a personal knowledge base (e.g. under a PARA-style `2-areas/`).
2. **Fill in the templates.** Work top-down:
   - `1-core/` — who you are. Optional; leave it blank and everything else still works.
   - `2-stack/` — your machine, shell, tools, hardware. Replace the example files with yours; delete what doesn't apply.
   - `3-taste/` — your writing voice, code style, naming. Swap the demo corpus for your own samples (see below).
   - `4-workflows/` — adjust the project and work-management conventions to how you actually work.
3. **Keep it current.** When a rule changes, edit the file. Agents read live, so there's nothing to rebuild for pointer-layer changes.

## Wiring a new agent runtime

Every runtime has a root instruction file it reads on startup — `AGENTS.md`, `CLAUDE.md`, `.codex/`, a `SKILL.md` tree, or something new. The contract is the same regardless of format:

1. Add one pointer to that root file:
   > Read `lib-me/readme.md` first, then `lib-me/4-workflows/agents-init.md`.
2. Let the agent follow [`4-workflows/agents-init.md`](4-workflows/agents-init.md). It specifies how to:
   - find lib-me and its reading order,
   - tell authoritative source files from derived artifacts,
   - convert workflow specs into the runtime's **native** skill format,
   - keep those derived skills in sync as the source changes.

The workflow files are the source of truth; an agent's native skills are generated copies. See the skill-to-source map in `agents-init.md`.

## The taste corpus

`3-taste/corpus/` holds short writing samples used as few-shot examples so agents can match your voice. The samples shipped in this public repo are **neutral demonstrations** — they show the mechanism, not anyone's real writing.

When you make lib-me yours, replace them with genuine samples of your own work. If you plan to share your filled-in copy publicly, keep the corpus tasteful and professional — agents only need enough to learn the cadence, not your private drafts.

## Contributing back

Improvements to the **structure** — the directory model, the bootstrap spec, the work-management system, clearer docs — are welcome via pull request. Keep personal content out of those PRs; lib-me ships as a template, not as anyone's filled-in instance.
