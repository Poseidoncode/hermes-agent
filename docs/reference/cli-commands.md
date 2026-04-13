# CLI Commands

Complete reference for Hermes Agent CLI commands.

## Session Commands

### /new

Start a fresh conversation session.

```
/new
```

Clears conversation history and starts new session.

### /reset

Reset current session completely.

```
/reset
```

Clears history, memory context, and tool state.

### /retry

Retry the last agent response.

```
/retry
```

Useful when agent made an error or you want to see alternative response.

### /undo

Undo the last turn.

```
/undo
```

Removes last user message and agent response from history.

### /compress

Compress conversation context.

```
/compress
```

Reduces token usage by compressing older messages.

### /usage

Show token usage statistics.

```
/usage
```

Displays:
- Total tokens used
- Cost estimate
- Token breakdown

### /insights

Show conversation insights.

```
/insights [--days N]
```

Options:
- `--days N` — Limit to last N days (default: 30)

## Configuration Commands

### /model

Switch AI model.

```
/model [provider:model]
/model list
/model anthropic/claude-3-5-sonnet-20241022
```

List available models with `/model list`.

### /personality

Set agent personality.

```
/personality [name]
/personality list
/personality helpful-assistant
```

### /config

Manage configuration.

```
/config show
/config set <key> <value>
/config get <key>
/config reset
/config edit
```

Examples:
```
/config show
/config set display.quiet_mode true
/config get model.provider
```

## Tools & Skills

### /tools

Manage tool configuration.

```
/tools
/tools enable <toolset>
/tools disable <toolset>
/tools list
```

### /skills

Browse and manage skills.

```
/skills
/skills search <query>
/skills install <skill>
/skills enable <skill>
/skills disable <skill>
```

### /mcp

Manage MCP servers.

```
/mcp
/mcp add <server>
/mcp remove <server>
/mcp list
/mcp status
```

## Memory Commands

### /memory

Manage memory.

```
/memory
/memory show
/memory search <query>
/memory clear
```

### /search

Search past conversations.

```
/search <query>
/search --days 30 <query>
```

Options:
- `--days N` — Limit search scope

## Platform Commands

### /gateway

Manage messaging gateway.

```
/gateway start
/gateway stop
/gateway status
/gateway setup
```

### /platforms

Show platform status.

```
/platforms
```

### /status

Show agent status.

```
/status
```

Displays:
- Current model
- Active tools
- Memory status
- Platform connections

## File Commands

### /context

Manage context files.

```
/context
/context add <file>
/context remove <file>
/context list
```

### /workspace

Set working directory.

```
/workspace [path]
```

Without path, shows current workspace.

## System Commands

### /help

Show help information.

```
/help
/help <command>
```

### /doctor

Run diagnostics.

```
/doctor
/doctor --verbose
```

Checks:
- Configuration
- API keys
- Tool availability
- System resources

### /logs

View logs.

```
/logs
/logs --tail 100
/logs --follow
```

Options:
- `--tail N` — Last N lines
- `--follow` — Stream new logs

### /debug

Toggle debug mode.

```
/debug
/debug on
/debug off
```

## Advanced Commands

### /interrupt

Interrupt current operation.

```
/interrupt
```

Stops agent mid-task.

### /background

Manage background tasks.

```
/background
/background list
/background stop <id>
/background logs <id>
```

### /delegate

Spawn subagent.

```
/delegate <prompt>
```

Creates isolated subagent for parallel work.

### /cron

Manage scheduled tasks.

```
/cron
/cron add <schedule> <command>
/cron list
/cron remove <id>
```

## Keybindings

| Key | Action |
|-----|--------|
| `Ctrl+C` | Interrupt |
| `Ctrl+D` | Exit |
| `Ctrl+L` | Clear screen |
| `Ctrl+R` | Search history |
| `Tab` | Autocomplete |
| `Esc` | Cancel input |

## Exit Commands

### /exit

Exit the CLI.

```
/exit
/quit
/bye
```

### /quit

Alias for `/exit`.

```
/quit
```

## Command Categories

| Category | Commands |
|----------|----------|
| Session | `/new`, `/reset`, `/retry`, `/undo`, `/compress` |
| Configuration | `/model`, `/personality`, `/config` |
| Tools & Skills | `/tools`, `/skills`, `/mcp` |
| Memory | `/memory`, `/search` |
| Platform | `/gateway`, `/platforms`, `/status` |
| Files | `/context`, `/workspace` |
| System | `/help`, `/doctor`, `/logs`, `/debug` |
| Advanced | `/interrupt`, `/background`, `/delegate`, `/cron` |
| Exit | `/exit`, `/quit`, `/bye` |

## Related

- [CLI Usage](../user-guide/cli.md)
- [Configuration](../user-guide/configuration.md)
