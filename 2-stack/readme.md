---
category: stack
tags: [stack, tool-discovery, environments, hardware, devices, frameworks, software]
---

# Stack

This directory catalogs the software, hardware, environments, and tools on this system.
Think of it as **tool discovery for agents** — it answers "what's available here?" before
agents suggest commands or configurations.

## Structure

- **agent-runtime.md** — shared utilities, execution environments, and the decision rule
  for where different kinds of tooling live
- **environments.md** — shell, terminal, multiplexer, and workflow environment
- **hardware.md** — workstation specs, GPU, compute resources
- **knowledge-base.md** — PARA-style knowledge-base layout and organizational rules
- **tmux-logging-setup.md** — terminal session logging and cleanup

Add your own as your stack grows — local model servers, RAG services, devices,
data pipelines. Each file describes one domain; list it here so agents can find it.

## How to Use

Agents should scan this directory (or relevant files) before suggesting:

1. **Commands** — know what's installed and where
2. **Tooling** — prefer existing tools over inventing new ones
3. **Environments** — use the right runtime for the job
4. **Hardware constraints** — don't suggest VRAM-heavy ops on a constrained GPU

Each file describes its domain in isolation. Start with the one matching your task.
