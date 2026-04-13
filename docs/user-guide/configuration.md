# Configuration

Configure Hermes Agent via YAML config file and environment variables.

## Config File

User configuration is stored at `~/.hermes/config.yaml`.

### Basic Structure

```yaml
# Hermes Agent Configuration
version: 5

# Model settings
model:
  provider: anthropic
  model: claude-opus-4.6
  max_tokens: 4096
  temperature: 0.7

# Display settings
display:
  quiet_mode: false
  save_trajectories: false
  skin: default
  tool_progress_command: true

# Tool settings
tools:
  enabled_toolsets:
    - core
    - web
    - files
  disabled_toolsets: []

# Memory settings
memory:
  enabled: true
  plugin: honcho
  auto_save: true

# Gateway settings
gateway:
  enabled_platforms:
    - telegram
  session_timeout: 3600

# Terminal backend
terminal:
  backend: local
  notify_on_complete: false
```

## Environment Variables

API keys and sensitive settings are stored in `~/.hermes/.env`.

### Provider Keys

```bash
# Anthropic
ANTHROPIC_API_KEY=sk-ant-...

# OpenAI
OPENAI_API_KEY=sk-...

# OpenRouter
OPENROUTER_API_KEY=...

# Google
GOOGLE_API_KEY=...
```

### Tool Keys

```bash
# Web search
TAVILY_API_KEY=...

# Browser automation
BROWSERBASE_API_KEY=...
BROWSERBASE_PROJECT_ID=...

# Firecrawl
FIRECRAWL_API_KEY=...
```

### Messaging Platforms

```bash
# Telegram
TELEGRAM_BOT_TOKEN=...

# Discord
DISCORD_BOT_TOKEN=...

# Slack
SLACK_BOT_TOKEN=...
SLACK_SIGNING_SECRET=...

# WhatsApp
WHATSAPP_PHONE_ID=...
WHATSAPP_TOKEN=...
```

## Configuration Options

### Model Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `model.provider` | `str` | `"anthropic"` | LLM provider |
| `model.model` | `str` | `"claude-opus-4.6"` | Model name |
| `model.max_tokens` | `int` | `4096` | Max response tokens |
| `model.temperature` | `float` | `0.7` | Sampling temperature |

### Display Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `display.quiet_mode` | `bool` | `false` | Suppress tool output |
| `display.save_trajectories` | `bool` | `false` | Save trajectories |
| `display.skin` | `str` | `"default"` | UI theme |
| `display.tool_progress_command` | `bool` | `true` | Show tool progress |

### Tool Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `tools.enabled_toolsets` | `list` | `["core"]` | Toolsets to enable |
| `tools.disabled_toolsets` | `list` | `[]` | Toolsets to disable |

### Memory Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `memory.enabled` | `bool` | `true` | Enable memory |
| `memory.plugin` | `str` | `"honcho"` | Memory plugin |
| `memory.auto_save` | `bool` | `true` | Auto-save memories |

### Terminal Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `terminal.backend` | `str` | `"local"` | Execution backend |
| `terminal.notify_on_complete` | `bool` | `false` | Notify on completion |

## Profiles

Hermes supports multiple isolated profiles:

```bash
# Use default profile
hermes

# Use specific profile
HERMES_PROFILE=work hermes
HERMES_PROFILE=personal hermes gateway
```

Each profile has its own:
- `~/.hermes/profiles/<name>/config.yaml`
- `~/.hermes/profiles/<name>/.env`
- Separate memory, sessions, skills

## Config Management Commands

```bash
# View current config
hermes config show

# Set a config value
hermes config set display.quiet_mode true

# Get a config value
hermes config get model.provider

# Reset to defaults
hermes config reset

# Edit config file
hermes config edit
```

## Migration

Config version is tracked in `hermes_cli/config.py`. When `_config_version` is bumped:
- Existing configs are migrated automatically
- New fields are added with defaults
- Deprecated fields are removed

Current version: 5

## Related

- [Environment Variables Reference](../reference/environment-variables.md)
- [CLI Commands](../reference/cli-commands.md)
