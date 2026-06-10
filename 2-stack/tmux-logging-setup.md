# Tmux Logging — Complete Guide

## Setup Overview

Tmux is configured to **log everything** — every keystroke, every command output, every screen update from every pane — into dedicated log files.

### How It Works

Tmux uses `pipe-pane` (a built-in feature) to redirect all output from each pane into a file. When you open a new pane or window, it automatically starts logging.

**Log directory:** `~/.tmux_logs/`
**Log format:** `{session_name}_{window_index}_{pane_index}.log`

Example files:
```
~/.tmux_logs/
├── work-0_0.log   # Session "work", window 0, pane 0
├── work-0_1.log   # Session "work", window 0, pane 1
├── dev-2_0.log    # Session "dev", window 2, pane 0
└── ...
```

### Security

- **Log directory:** `chmod 700` (owner-only access)
- **Log files:** `chmod 600` (owner-only read/write)
- **No sharing:** Other users on the system cannot read your logs
- ⚠️ **Be aware:** Logs contain **everything** displayed — including any passwords, tokens, or secrets you type or display. Only log sessions you're okay with recording.

### Automatic Cleanup

Logs older than **90 days** are automatically deleted every **Monday at 3:00 AM** via cron.

**Manual cleanup anytime:**
```bash
~/.local/bin/tmux-log-cleanup.sh
```

---

## Usage

### New Sessions — Automatic Logging

Every new tmux session automatically logs all panes. Just create a session and go:

```bash
tmux new -s mysession    # Everything in this session is logged
```

Each pane you create (via `Ctrl+b %` or `Ctrl+b "`) gets its own log file.

### Existing Sessions

If you attach to a running session, logging is automatically enabled for all existing panes thanks to tmux hooks.

### Viewing a Log

```bash
# View in real-time
tail -f ~/.tmux_logs/my-session_0_0.log

# Search through it
grep "error" ~/.tmux_logs/my-session_0_0.log

# View with less (scrollable)
less ~/.tmux_logs/my-session_0_0.log
```

### Stopping Logging on a Single Pane

To stop logging a specific pane (e.g., a pager or viewer you don't need to record):

```bash
# In tmux, select the pane, then run:
tmux pipe-pane -t @CURRENT_PANE_ID
```

This toggles `pipe-pane` off for that pane only.

### Checking What's Being Logged

```bash
# List all log files for a session
ls -la ~/.tmux_logs/ | grep "sessionname"

# See active pipe-pane connections (non-empty = logging active)
tmux list-panes -a -F '#{pane_id}  pipe-pane=#{pane_active_pipe}'
```

### Deleting Old Logs Manually

```bash
# Remove logs older than 90 days
~/.local/bin/tmux-log-cleanup.sh

# Remove a specific session's logs
rm ~/.tmux_logs/sessionname_*.log
```

---

## Configuration Files

| File | Purpose |
|------|---------|
| `~/.tmux.conf` | Tmux config — hooks that auto-log new panes |
| `~/.local/bin/tmux-log-cleanup.sh` | 90-day cleanup script |
| `~/.tmux_logs/` | Log directory (chmod 700) |

### Cron Job

```
0 3 * * 1 ~/.local/bin/tmux-log-cleanup.sh
```

Runs every Monday at 3:00 AM.

### tmux.conf Logging Section

```bash
# Session created → start logging for all existing panes
set-hook -g session-created "run-shell '$HOME/.local/bin/tmux-log-start.sh'"

# Pane split → start logging the new pane
set-hook -g after-split-window "pipe-pane -o 'cat >>$HOME/.tmux_logs/#{session_name}_#{window_index}_#{pane_index}.log'"
```

> **Note:** `new-pane` and `create-window` hooks require tmux 4.0+. This setup uses tmux 3.6-compatible hooks (`session-created` + `after-split-window`).

---

## Troubleshooting

### Log files aren't being created

1. **Restart tmux:** After updating `~/.tmux.conf`, restart tmux completely (exit all sessions or run `tmux kill-server` then start a new session)
2. **Check hooks:** `tmux show-hook -g new-pane` should show the pipe-pane command
3. **Verify directory:** `ls -la ~/.tmux_logs/` should exist with `drwx------` permissions

### Logs seem empty

The log only records output that appears on screen. If you're in an interactive program that buffers output (like `top`, `htop`, `vim`), the log may not show everything. Shell output (commands, piped output, `cat`, etc.) is logged fully.

### Want to stop logging entirely

Edit `~/.tmux.conf` and comment out the `set-hook` lines, then restart tmux.

### Accidentally logged sensitive data

Immediately stop the pane logging:
```bash
# Get the pane ID first
tmux list-panes
tmux pipe-pane -t @PANE_ID
```

The `pipe-pane` command without arguments **toggles it off**. Then delete the affected log file. Be aware any copies (backups, sync) may also contain the data.

---

## Notes

- Tmux 3.6 does **not** have a built-in `log-file` option. This setup uses `pipe-pane` which is the standard tmux mechanism.
- Hooks `new-pane` and `create-window` were added in tmux 4.0. This config uses tmux 3.6 hooks (`session-created` + `after-split-window`).
- If you upgrade to tmux 4.0+, the config should be updated to use `new-pane` instead of `session-created` for cleaner logging.
- Logs grow continuously while the pane is active. The 90-day cleanup handles old files, but large sessions can accumulate significant log data.
- Log file permissions (`600`) prevent other users from reading, but **root** can always access them.
- If you use `scp` or `rsync` to back up `~/.tmux_logs/`, be mindful of the sensitive data they contain.
