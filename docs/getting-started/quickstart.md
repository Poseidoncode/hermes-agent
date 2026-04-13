# Quickstart

Get Hermes Agent running in 2 minutes.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Works on Linux, macOS, WSL2, and Android (Termux).

### Post-Install

```bash
# Reload shell
source ~/.bashrc    # or: source ~/.zshrc

# Start Hermes
hermes
```

## First Conversation

1. **Run setup wizard** (first time only):
```bash
hermes setup
```

2. **Choose your model**:
```bash
hermes model
```

3. **Start chatting**:
```bash
hermes
```

## Basic Commands

| Command | Description |
|---------|-------------|
| `/model` | Change AI model |
| `/tools` | Configure tools |
| `/skills` | Browse skills |
| `/memory` | Manage memory |
| `/help` | Show help |
| `/exit` | Exit Hermes |

## Next Steps

### Enable Tools

```bash
hermes tools
```

Enable web search, browser automation, file tools, etc.

### Install Skills

```bash
/skills
/skills install <skill-name>
```

Browse and install community skills.

### Connect Messaging

```bash
hermes gateway setup
hermes gateway start
```

Connect Telegram, Discord, Slack, or WhatsApp.

## Configuration

### Set API Keys

```bash
# Edit .env file
nano ~/.hermes/.env

# Add your keys
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...
```

### Customize Settings

```bash
# View config
hermes config show

# Change setting
hermes config set display.quiet_mode true
```

## Troubleshooting

### Check Status

```bash
hermes doctor
```

### View Logs

```bash
hermes logs --tail 50
```

### Get Help

- **Docs:** https://hermes-agent.nousresearch.com/docs/
- **Discord:** https://discord.gg/NousResearch
- **Issues:** https://github.com/NousResearch/hermes-agent/issues

## What's Next?

- [Installation Guide](./installation.md) — Detailed install instructions
- [CLI Usage](../user-guide/cli.md) — Learn all CLI commands
- [Configuration](../user-guide/configuration.md) — Customize Hermes
- [Features](../features/tools.md) — Explore tools and skills
