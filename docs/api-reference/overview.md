# API Reference

Complete API documentation for Hermes Agent modules and classes.

## Core Modules

| Module | Description |
|--------|-------------|
| [`AIAgent`](./ai-agent.md) | Main agent class for conversation management |
| [`ToolRegistry`](./tool-registry.md) | Central tool registration and dispatch system |
| [`HermesCLI`](./cli.md) | Interactive CLI orchestrator |
| [`Gateway`](./gateway.md) | Messaging platform integration |

## Module Categories

### Agent Core

- **AIAgent** — Conversation loop, tool calling, response management
- **ContextCompressor** — Automatic context compression
- **PromptBuilder** — System prompt assembly
- **PromptCaching** — Anthropic prompt caching
- **MemoryManager** — Memory context building

### Tools

- **ToolRegistry** — Tool schema collection and dispatch
- **TerminalTool** — Terminal orchestration
- **FileTools** — File read/write/search/patch
- **WebTools** — Web search and extraction
- **BrowserTool** — Browser automation
- **MCPTool** — Model Context Protocol client
- **DelegateTool** — Subagent delegation

### CLI

- **HermesCLI** — CLI main class
- **SlashCommands** — Command definitions and dispatch
- **SkinEngine** — Theme and visual customization
- **Setup** — Interactive setup wizard

### Gateway

- **SessionStore** — Conversation persistence
- **Platform Adapters** — Telegram, Discord, Slack, WhatsApp, Signal
- **Hooks** — Message processing pipeline

## Usage Examples

### Basic Agent Usage

```python
from run_agent import AIAgent

agent = AIAgent(
    model="anthropic/claude-opus-4.6",
    max_iterations=90,
    quiet_mode=False
)

response = agent.chat("What can you help me with?")
print(response)
```

### Tool Registry Usage

```python
from tools.registry import registry

definitions = registry.get_definitions(
    tool_names={"terminal", "file_read", "web_search"},
    quiet=False
)

result = registry.dispatch(
    name="terminal",
    args={"command": "ls -la"},
    task_id="task_123"
)
```

### CLI Usage

```python
from cli import HermesCLI

cli = HermesCLI()
cli.run()
```

## Related Documentation

- [Architecture Overview](../architecture/overview.md)
- [Adding Tools](../developer-guide/adding-tools.md)
- [Configuration](../user-guide/configuration.md)
