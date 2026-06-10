# Documentation: Local Dev Work Management System

## Overview
Work is tracked in docs, not source comments. Keep `0-wm` as the system of record so a project can be dropped and resumed cleanly.

This is the full work-management guide. Use the lite quick-start when you only need `todo.md`-first work with optional milestone layer.
Check `lite-quick-start.md` first unless the work truly needs the additional hierarchy below.

## System Realities
- The code is the body; `0-wm` is the brain.
- Context should survive long gaps.
- Keep the workflow light unless complexity justifies more structure.
- TODO, FIXME, DECIDE, and IDEA live in docs.

## Document Types in `0-wm`
- `roadmap.md`: optional coarse sequence above milestones. Roadmap items use `r0x`.
- `milestones.md`: optional milestone list. Milestones use `m0x`.
- `initiatives.md`: grouped workstreams. Initiatives use `iv`.
- `features.md`: focused deliverables. Features use `ft`.
- `todo.md`: canonical home for active tasks and decisions. Scoped TODOs use `m0x-t00x`; unclassified TODOs use `m00-t00x`.
- `sprint.md`: optional focus list over `todo.md`. Sprint items use `s0x`.
- `decisions.md`: resolved decisions. Optional.
- `ideas.md`: raw ideas. Optional.
- `status.md`: current state, blockers, and handoff context.
- `log.md`: short timestamped session notes.
- `design*.md` and `implementation-plan*.md`: deeper detail when needed.

## Rules
- The only non-optional files are `status.md` and `log.md`.
- All files are lowercase.
- `todo.md` is the canonical active task and decision list.
- TODO entries must include the literal `TODO`.
- DECIDE entries stay in `todo.md` until resolved, then move to `decisions.md`.
- TODOs may carry `r`, `m`, `iv`, and `ft` keys as needed.
- Keys precede the description and are wrapped in curly braces.
- `sprint.md` is optional, indexes `todo.md`, and does not replace it.
- Sprint items are markdown tasks like `- [ ] s01 - {m01-t01} - task description.`

## Choosing Structure
- One thing to do: one TODO in `todo.md`.
- Coarse sequence needed: add `roadmap.md`.
- Outcome checkpoints needed: add `milestones.md`.
- Three or more related features: add `initiatives.md`.
- A focused deliverable: add `features.md`.
- A decision: add `DECIDE` to `todo.md`.
- A raw note: add `IDEA` to `ideas.md`.
- Prioritization for the current iteration: add `sprint.md`.

## Quick Formats
- `roadmap.md`: `- [ ] **{r01} Roadmap title.** Short roadmap description.`
- `milestones.md`: `- [ ] **{m01} Milestone title.** Short milestone description.`
- `initiatives.md`: `- [ ] **{iv01} {r07} Initiative title.** Short initiative description.`
- `features.md`: `- [ ] **{ft01} {iv01} {m07} Feature title.** Short feature description.`
- `todo.md`: `- [ ] TODO **{m05} {ft03} Task description.**`
- `todo.md` decision: `- DECIDE **{m07} {iv01} {ft02} Decision statement.**`
- `decisions.md`: `- YYYYMMDDHHSS **{ADR-NNNN}** Decision: {decision text}.`
- `ideas.md`: `- Idea description.`

## Sprint
- A sprint is a temporary prioritized working set.
- Copy only the TODOs you intend to work now.
- Use `s01`, `s02`, etc. and keep ordering strict.
- When done, mark the source TODOs complete in `todo.md`, update `status.md`, and archive the sprint file if needed.

## Work Creation
1. Check existing `r*`, `m*`, `iv*`, and `ft*` structure first.
2. If there is no structure and the work is small, add a TODO directly.
3. If it is a new coarse sequence, add `roadmap.md` first.
4. If it is a new milestone, add `milestones.md` and link the TODO to `m*`.
5. If it is a cluster of features, add `initiatives.md` and `features.md`.
6. If it is a decision, write `DECIDE` in `todo.md`.
7. If it is immediate work, add it to `todo.md` and optionally `sprint.md`.

## LLM Assistants
- Scan `0-wm/` for context.
- Resolve `DECIDE` items by drafting an ADR, updating `decisions.md`, and removing the `DECIDE` from `todo.md`.
- Move promoted `IDEA` items out of `ideas.md`.
- Log completed tasks and notable state changes in `log.md`.
