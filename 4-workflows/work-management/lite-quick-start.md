# Work Management Lite: Quick Start

Every project has a `0-wm/` folder. These are the files inside it and what to do with them.

## Workflow

1. Start in `todo.md`. That is the simplest working setup.
2. If task grouping helps, add `milestones.md`. In that mode, `m00` is a virtual milestone for tasks that are not assigned to a real milestone yet, and real milestone tasks use IDs like `m01-t01`.
3. Work through the tasks. Log notable things in `log.md`.
4. Update `status.md` at the end of each session.
5. If you need more sophisticated hierarchy, switch to [full-system.md](full-system.md).


## Files

### `milestones.md` [Optional]

The sequence of outcomes that defines "done" for the project, if you need one.
Each item is one sentence describing a real, verifiable outcome — not a task. Number them in the order they should be completed: m01, m02, m03. Work on m02 only after m01 is finished.

```
- [ ] m01 Database schema finalized and migrations running cleanly.
- [ ] m02 API endpoints wired up and returning correct responses.
- [ ] m03 UI connected to API, end-to-end flow working in staging.
```

### `todo.md`

The active task list under whichever milestone you're working on.
Tasks are markdown checkboxes. Reference the parent milestone in the task id so it's clear what each task is for. If a task does not belong to a real milestone, use `m00` in the id. `m00` means "not grouped under a real milestone yet." Check tasks off as you go.

```
- [ ] TODO m01-t01 - Set up the users table
- [ ] TODO m01-t02 - Write the migration script
- [x] TODO m01-t03 - Review schema with the team
- [ ] TODO m00-t01 - Triage the incoming request
```

### `status.md`

Where things stand right now.
Update this whenever the situation changes: a blocker appears, a phase ends, you hand off to someone. Keep it short. It's for someone (or future-you) opening the project cold.

### `log.md`

A running timestamped journal.
Append a line when something notable happens: a decision made, a task finished, a session ending. Format: `YYYYMMDDHHSS - What happened.`

### `decisions.md` [Optional]

Resolved decisions, one per line.
Create this once you have a few decisions worth tracking.

```
- Decision: Use Postgres, not SQLite. Needed for multi-user support.
```

### `ingest.md` [Optional]

A dumping ground for anything that needs to be captured right now. Tasks, ideas, questions, links, half-formed thoughts — doesn't matter. Just get it out of your head and into the file. Sort it into the right place later.
