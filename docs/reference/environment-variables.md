# Environment Variables

Complete reference for Hermes Agent environment variables.

## Provider API Keys

### Anthropic

| Variable | Description | Required |
|----------|-------------|----------|
| `ANTHROPIC_API_KEY` | Anthropic API key | Yes (for Anthropic models) |

### OpenAI

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENAI_API_KEY` | OpenAI API key | Yes (for OpenAI models) |
| `OPENAI_BASE_URL` | Custom OpenAI endpoint | No |

### OpenRouter

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENROUTER_API_KEY` | OpenRouter API key | Yes (for OpenRouter) |

### Google

| Variable | Description | Required |
|----------|-------------|----------|
| `GOOGLE_API_KEY` | Google AI API key | Yes (for Gemini) |
| `GOOGLE_APPLICATION_CREDENTIALS` | Service account JSON path | For Vertex AI |

### Other Providers

| Variable | Description | Required |
|----------|-------------|----------|
| `ZAI_API_KEY` | z.ai/GLM API key | Yes (for z.ai) |
| `MOONSHOT_API_KEY` | Moonshot/Kimi API key | Yes (for Kimi) |
| `MINIMAX_API_KEY` | MiniMax API key | Yes (for MiniMax) |

## Tool API Keys

### Web Search

| Variable | Description | Required |
|----------|-------------|----------|
| `TAVILY_API_KEY` | Tavily search API key | For web search tool |
| `SERPAPI_API_KEY` | SerpAPI key | Alternative search |

### Browser Automation

| Variable | Description | Required |
|----------|-------------|----------|
| `BROWSERBASE_API_KEY` | Browserbase API key | For browser tool |
| `BROWSERBASE_PROJECT_ID` | Browserbase project ID | For browser tool |
| `FIRECRAWL_API_KEY` | Firecrawl API key | For web scraping |

### Code Execution

| Variable | Description | Required |
|----------|-------------|----------|
| `E2B_API_KEY` | E2B sandbox API key | For E2B backend |

### Image Generation

| Variable | Description | Required |
|----------|-------------|----------|
| `REPLICATE_API_KEY` | Replicate API key | For image generation |
| `STABILITY_API_KEY` | Stability AI key | For Stable Diffusion |

### Transcription & TTS

| Variable | Description | Required |
|----------|-------------|----------|
| `ELEVENLABS_API_KEY` | ElevenLabs API key | For TTS |
| `DEEPGRAM_API_KEY` | Deepgram API key | For transcription |

## Messaging Platforms

### Telegram

| Variable | Description | Required |
|----------|-------------|----------|
| `TELEGRAM_BOT_TOKEN` | Telegram bot token | For Telegram gateway |

### Discord

| Variable | Description | Required |
|----------|-------------|----------|
| `DISCORD_BOT_TOKEN` | Discord bot token | For Discord gateway |
| `DISCORD_CLIENT_ID` | Discord client ID | For OAuth |

### Slack

| Variable | Description | Required |
|----------|-------------|----------|
| `SLACK_BOT_TOKEN` | Slack bot token | For Slack gateway |
| `SLACK_SIGNING_SECRET` | Slack signing secret | For request verification |

### WhatsApp

| Variable | Description | Required |
|----------|-------------|----------|
| `WHATSAPP_PHONE_ID` | WhatsApp phone ID | For WhatsApp gateway |
| `WHATSAPP_TOKEN` | WhatsApp API token | For WhatsApp gateway |

### Signal

| Variable | Description | Required |
|----------|-------------|----------|
| `SIGNAL_PHONE_NUMBER` | Signal phone number | For Signal gateway |
| `SIGNAL_PASSWORD` | Signal password | For Signal gateway |

### Email

| Variable | Description | Required |
|----------|-------------|----------|
| `EMAIL_ADDRESS` | Email address | For email gateway |
| `EMAIL_PASSWORD` | Email password | For email gateway |
| `SMTP_SERVER` | SMTP server | For email gateway |

## Configuration

### Profile

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_PROFILE` | Active profile name | default |
| `HERMES_HOME` | Custom HERMES_HOME path | `~/.hermes` |

### Terminal Backend

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_TERMINAL_BACKEND` | Default terminal backend | `local` |
| `HERMES_DOCKER_IMAGE` | Docker image for backend | `python:3.11-slim` |
| `HERMES_SSH_HOST` | SSH host for backend | - |
| `HERMES_SSH_USER` | SSH user for backend | - |

### Display

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_QUIET_MODE` | Suppress tool output | `false` |
| `HERMES_SAVE_TRAJECTORIES` | Save trajectories | `false` |
| `HERMES_SKIN` | UI theme/skin | `default` |

### Memory

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_MEMORY_ENABLED` | Enable memory | `true` |
| `HERMES_MEMORY_PLUGIN` | Memory plugin | `honcho` |
| `HERMES_MEMORY_AUTO_SAVE` | Auto-save memories | `true` |

### Security

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_APPROVAL_MODE` | Command approval mode | `dangerous` |
| `HERMES_ALLOWED_COMMANDS` | Allowed commands (comma-separated) | - |
| `HERMES_BLOCKED_COMMANDS` | Blocked commands (comma-separated) | - |

### Debugging

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_DEBUG` | Enable debug mode | `false` |
| `HERMES_LOG_LEVEL` | Logging level | `INFO` |
| `HERMES_LOG_FILE` | Log file path | `~/.hermes/logs/hermes.log` |

### Performance

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_MAX_ITERATIONS` | Max tool iterations | `90` |
| `HERMES_CONTEXT_LIMIT` | Token context limit | Auto-detect |
| `HERMES_TEMPERATURE` | Model temperature | `0.7` |

### Gateway

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_GATEWAY_PORT` | Gateway HTTP port | `8080` |
| `HERMES_GATEWAY_HOST` | Gateway host | `0.0.0.0` |
| `HERMES_SESSION_TIMEOUT` | Session timeout (seconds) | `3600` |

### Messaging CWD

| Variable | Description | Default |
|----------|-------------|---------|
| `MESSAGING_CWD` | Working directory for messaging mode | Home directory |

### Background Notifications

| Variable | Description | Default |
|----------|-------------|---------|
| `HERMES_BACKGROUND_NOTIFICATIONS` | Background process notifications | `all` |

Options: `all`, `result`, `error`, `off`

## Setup

### Using .env File

Create `~/.hermes/.env`:

```bash
# Provider keys
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...

# Tool keys
TAVILY_API_KEY=...
BROWSERBASE_API_KEY=...

# Messaging
TELEGRAM_BOT_TOKEN=...

# Configuration
HERMES_QUIET_MODE=false
HERMES_DEBUG=false
```

### Export in Shell

```bash
export ANTHROPIC_API_KEY=sk-ant-...
export OPENAI_API_KEY=sk-...
export HERMES_DEBUG=1
```

### Verify Configuration

```bash
# Check loaded env vars
hermes doctor

# Show config
hermes config show
```

## Precedence

Environment variables are loaded in this order (later overrides earlier):

1. System environment
2. `~/.hermes/.env`
3. Project root `.env` (dev fallback)
4. Profile-specific `.env`

## Security Notes

- Never commit `.env` files to version control
- Use separate keys for development and production
- Rotate keys periodically
- Use profile-specific keys for isolation

## Related

- [Configuration](../user-guide/configuration.md)
- [Security](../user-guide/security.md)
