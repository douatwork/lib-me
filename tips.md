# Tips & Tricks

Optional ways to make lib-me fit you better. None are required — lib-me works out of
the box. Pick what helps.

---

## Give your instance a name

lib-me ships generic. Give your copy an identity — agents read better when they refer to
*your* manual by name instead of "the template." Pick a name, then use it in the readme
intro and in the one-line pointer in each agent's root file ("Read `<name>/AGENTS.md`
first"). It's cosmetic, but it makes the system feel like yours.

---

## Rename directories to fit how you think

The directory names are yours. If `taste/` doesn't match how you think about it,
rename it — `style/`, `voice/`, whatever fits. lib-me doesn't depend on the names, only
on the pointers staying consistent: update the readme map, the reading order in
`agents-init.md`, and any frontmatter that references the old path.

---

## Invest in your voice guide last, not first

`taste/writing-style.md` is the highest-leverage taste file — but it's only as good as
the examples behind it. Populate `taste/corpus/` with real samples of your writing
*first*, then spend your best model's tokens distilling `writing-style.md` from them.
Distilling a voice from a full corpus is worth a premium model; writing corpus stubs
isn't.

---

## Keep generated skills in sync with lib-me

Agents use lib-me in two ways, and only one of them needs syncing:

- **Read live.** `core/`, `stack/`, `taste/`, `README.md`, and `AGENTS.md` are read
  fresh each session. Edit them and the next session sees the change. Nothing to do.
- **Generated once.** Some agents convert the specs in `workflows/` into their own
  native skills or instruction files. Those are copies, so they go stale when you edit
  the source. They have to be regenerated.

This tip is about automating that regeneration — rebuilding an agent's generated skills
when the lib-me source behind them changes, with no custom tooling.

The trick is to use git as the version marker. lib-me lives in git, so "the version an
agent last saw" is just a commit hash:

1. **Anchor on the lib-me directory, not repo HEAD.** The current version is the last
   commit that touched lib-me specifically — so unrelated edits elsewhere in a larger
   repo don't trigger a needless resync.
2. **Each agent stores its last-synced commit** in a state file kept *outside* the
   lib-me repo. No state (or first run) means a full regeneration.
3. **Detect change by diff.** Last-synced commit equals current → done. Different → the
   path-scoped diff lists exactly which source files changed.
4. **Regenerate only what changed,** mapping each changed source to the agent's
   generated artifacts, then lint the result.
5. **Promote or roll back.** Clean lint → replace the old artifacts and advance the
   stored commit. Failure → keep the old version, leave the state file untouched.

Sync only ever *reads* lib-me; it *writes* only the agent's own skills and state file.
The full contract — reading order, the skill-to-source map, and edge cases like a squash
or rebase orphaning the old hash (which falls back to full regeneration) — lives in
[`workflows/agents-init.md`](workflows/agents-init.md).

The detection is a handful of git commands, but regeneration is runtime-specific: every
agent has its own skill format and config location. So hand the job to the agent itself.
Copy this prompt to the agent you want kept in sync, filling in the two bracketed values:

```text
Read `[path-to-lib-me]/workflows/agents-init.md` — it specifies how this system
versions and syncs generated artifacts. Then set up an automated resync for yourself,
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
   format. Files that are read live — core, stack, taste, README, AGENTS — need no regeneration.
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
