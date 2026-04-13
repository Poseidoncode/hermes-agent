# Troubleshooting

Common issues and solutions for Hermes Agent.

## Installation Issues

### pip install fails

**Problem:** Installation fails with dependency conflicts.

**Solution:**
```bash
# Use uv instead of pip
curl -LsSf https://astral.sh/uv/install.sh | sh
uv venv venv --python 3.11
source venv/bin/activate
uv pip install -e ".[all]"
```

### Missing system dependencies

**Problem:** Build errors for packages like `cryptography`, `lxml`.

**Solution:**
```bash
# Ubuntu/Debian
sudo apt-get install python3-dev libffi-dev libssl-dev

# macOS
xcode-select --install
brew install libffi openssl

# Then reinstall
uv pip install -e ".[all]"
```

## Model Connection Issues

### API key not found

**Problem:** `Error: ANTHROPIC_API_KEY not set`

**Solution:**
```bash
# Set in .env file
echo "ANTHROPIC_API_KEY=sk-ant-..." >> ~/.hermes/.env

# Or export temporarily
export ANTHROPIC_API_KEY=sk-ant-...

# Verify
hermes config show
```

### Rate limit errors

**Problem:** `429 Too Many Requests`

**Solution:**
1. Reduce `max_iterations` in config
2. Add request delays
3. Upgrade API plan
4. Switch to different provider

```yaml
model:
  max_iterations: 50  # Reduce from 90
```

### Model not found

**Problem:** `Error: Model 'claude-opus-4.6' not found`

**Solution:**
```bash
# List available models
hermes models list

# Switch model
hermes model anthropic/claude-3-5-sonnet-20241022
```

## Tool Execution Issues

### Tool unavailable

**Problem:** Tool shows as unavailable in `hermes tools`

**Solution:**
```bash
# Check required env vars
hermes doctor

# Set missing keys
hermes config set tools.env.API_KEY value

# Enable toolset
hermes tools enable web
```

### Terminal backend fails

**Problem:** Commands fail to execute in terminal

**Solution:**
```bash
# Check backend status
hermes status

# Switch to local backend
hermes config set terminal.backend local

# For Docker backend
docker ps  # Verify daemon running
```

### Permission denied

**Problem:** `PermissionError` when executing commands

**Solution:**
```bash
# Check file permissions
ls -la ~/.hermes/

# Fix ownership
chown -R $USER:$USER ~/.hermes/

# For Docker backend
docker run --user $(id -u):$(id -g) ...
```

## Memory Issues

### Memory not saving

**Problem:** Memories not persisting between sessions

**Solution:**
```bash
# Check memory plugin
hermes config get memory.plugin

# Verify plugin installed
ls ~/.hermes/plugins/memory/

# Reinstall plugin
hermes plugins install honcho
```

### Session search slow

**Problem:** Session search takes too long

**Solution:**
```bash
# Rebuild FTS index
hermes memory rebuild-index

# Limit search scope
/search --days 30 query
```

## Gateway Issues

### Gateway won't start

**Problem:** `hermes gateway start` fails

**Solution:**
```bash
# Check platform config
hermes gateway status

# Verify credentials
hermes doctor

# Check port conflicts
lsof -i :8080

# Use different port
hermes gateway start --port 8081
```

### Messages not delivered

**Problem:** Gateway running but messages not received

**Solution:**
```bash
# Check webhook URL
hermes gateway webhook-info

# Verify platform credentials
hermes config show | grep telegram

# Test connection
hermes gateway test telegram
```

## Performance Issues

### Slow response times

**Problem:** Agent takes too long to respond

**Solution:**
1. Enable prompt caching
2. Reduce context size
3. Use faster model
4. Disable unused tools

```yaml
display:
  quiet_mode: true  # Reduce output

tools:
  disabled_toolsets:
    - browser  # Disable heavy tools
```

### High memory usage

**Problem:** Agent uses excessive memory

**Solution:**
```bash
# Limit conversation history
/compress

# Disable trajectory saving
hermes config set display.save_trajectories false

# Restart with fresh session
/reset
```

## Profile Issues

### Profile not switching

**Problem:** `HERMES_PROFILE` has no effect

**Solution:**
```bash
# Verify profile exists
ls ~/.hermes/profiles/

# Create profile
hermes profiles create work

# Switch profile
export HERMES_PROFILE=work
hermes
```

### Profile data missing

**Problem:** Config/memories not found in profile

**Solution:**
```bash
# Check profile path
echo $HERMES_PROFILE

# Verify files exist
ls ~/.hermes/profiles/$HERMES_PROFILE/

# Migrate data if needed
hermes profiles import
```

## Debug Mode

Enable debug logging for detailed diagnostics:

```bash
# Set debug level
export HERMES_DEBUG=1
hermes

# Or use doctor command
hermes doctor --verbose
```

Debug logs are saved to `~/.hermes/logs/hermes.log`.

## Getting Help

- **Documentation:** https://hermes-agent.nousresearch.com/docs/
- **Discord:** https://discord.gg/NousResearch
- **Issues:** https://github.com/NousResearch/hermes-agent/issues
- **Discussions:** https://github.com/NousResearch/hermes-agent/discussions

## Related

- [Environment Variables](../reference/environment-variables.md)
- [CLI Commands](../reference/cli-commands.md)
