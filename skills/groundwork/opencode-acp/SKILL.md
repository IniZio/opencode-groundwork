---
name: opencode-acp
description: Control another OpenCode instance via the Agent Client Protocol (ACP). Start an ACP server and send prompts using the `opencode` CLI.
---

# OpenCode ACP

Control another OpenCode instance via ACP (Agent Client Protocol).

## Quick Start

### 1. Start an ACP server

Use `pty_spawn` to start the server on a fixed port:

```
pty_spawn(command: "opencode", args: ["acp", "--port", "9090"], title: "ACP Server")
```

Wait ~2 seconds for it to be ready.

### 2. Send a prompt via `opencode run --attach`

```
bash(command: 'opencode run "Your prompt here" --attach http://localhost:9090 -m kimi-for-coding/k2p5 --format json --dir /path/to/project')
```

Use `--format json` for structured output (easier to parse). Use `-m provider/model` to select a model. Omit `-m` to use the server's default.

### 3. Stop the server

```
pty_kill(id: "pty_xxxxxx", cleanup: true)
```

## Common Options

| Flag | Purpose |
|------|---------|
| `--attach http://localhost:9090` | Target ACP server |
| `-m provider/model` | Select model (e.g. `kimi-for-coding/k2p5`) |
| `--dir /path/to/project` | Working directory for the prompt |
| `--format json` | Output raw JSON events instead of formatted text |
| `--agent <agent>` | Select agent |
| `--session <id>` | Resume an existing session |
| `--continue` | Continue the last session |

## Model Selection

To use the **Kimi 2.5** model on the `kimi-for-coding` provider:

```
opencode run "Your prompt" --attach http://localhost:9090 -m kimi-for-coding/k2p5
```

## What NOT to Do

- Do **not** use the old `process.write` / `process.poll` JSON-RPC workflow described in older versions of this skill. `opencode acp` is an **HTTP server**, not a stdin/stdout process.
- Do **not** forget `--dir` if the prompt needs file-system context.
- Do **not** send prompts immediately after `pty_spawn` — allow 1–2 seconds for the server to bind.

## Example: Full Interaction

```
# 1. Start server
pty_spawn(command: "opencode", args: ["acp", "--port", "9090"], title: "ACP Server")

# 2. Wait briefly
bash(command: "sleep 2")

# 3. Send prompt with Kimi 2.5
bash(command: 'opencode run "List files" --attach http://localhost:9090 -m kimi-for-coding/k2p5 --dir /Users/newman/project')

# 4. Clean up
pty_kill(id: "<session-id>", cleanup: true)
```
