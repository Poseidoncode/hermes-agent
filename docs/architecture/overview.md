# Architecture Overview

System architecture and component relationships for Hermes Agent.

## System Architecture

Hermes Agent is built with a modular architecture that separates concerns across multiple layers.

```
┌─────────────────────────────────────────────────────────────┐
│                    Messaging Platforms                       │
│   Telegram │ Discord │ Slack │ WhatsApp │ Signal │ CLI      │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                      Gateway Layer                           │
│   Platform Adapters │ Session Store │ Hooks │ Delivery      │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                      Agent Core                              │
│   AIAgent │ Prompt Builder │ Context Compressor │ Memory    │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                     Tool System                              │
│   Registry │ Terminal │ Files │ Web │ Browser │ MCP │ Skills│
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  Execution Environments                      │
│   Local │ Docker │ SSH │ Modal │ Daytona │ Singularity      │
└─────────────────────────────────────────────────────────────┘
```

## Component Relationships

### Agent Core

The agent core manages the conversation loop and decision-making:

- **AIAgent** (`run_agent.py`) — Main conversation orchestrator
- **PromptBuilder** (`agent/prompt_builder.py`) — System prompt assembly
- **ContextCompressor** (`agent/context_compressor.py`) — Auto context compression
- **PromptCaching** (`agent/prompt_caching.py`) — Anthropic prompt caching
- **MemoryManager** (`agent/memory_manager.py`) — Memory context building

### Tool System

Tools are registered centrally and dispatched dynamically:

- **Registry** (`tools/registry.py`) — Central tool registration
- **Model Tools** (`model_tools.py`) — Tool orchestration and discovery
- **Toolsets** (`toolsets.py`) — Tool grouping and configuration

### CLI Layer

The interactive terminal interface:

- **HermesCLI** (`cli.py`) — CLI orchestrator
- **Commands** (`hermes_cli/commands.py`) — Slash command registry
- **Skin Engine** (`hermes_cli/skin_engine.py`) — Theme system
- **Setup** (`hermes_cli/setup.py`) — Interactive wizard

### Gateway Layer

Messaging platform integration:

- **Gateway** (`gateway/run.py`) — Main loop and dispatch
- **Session** (`gateway/session.py`) — Conversation persistence
- **Platforms** (`gateway/platforms/`) — Platform adapters

### Execution Environments

Terminal backends for tool execution:

- **Local** — Direct local execution
- **Docker** — Containerized execution
- **SSH** — Remote server execution
- **Modal** — Serverless cloud execution
- **Daytona** — Cloud development environments
- **Singularity** — HPC container execution

## Data Flow

### Conversation Flow

1. User sends message via CLI or messaging platform
2. Gateway/CLI receives and validates message
3. AIAgent processes message with conversation history
4. PromptBuilder assembles system prompt with context
5. LLM returns response with optional tool calls
6. ToolRegistry dispatches tool calls to handlers
7. Tools execute in appropriate environment
8. Results returned to LLM for next iteration
9. Final response sent to user

### Tool Execution Flow

```
User Request
    ↓
AIAgent.run_conversation()
    ↓
LLM → Tool Call
    ↓
ToolRegistry.dispatch()
    ↓
Tool Handler
    ↓
Environment (Local/Docker/SSH/etc.)
    ↓
Result → LLM
    ↓
Final Response
```

## Key Design Decisions

### Modular Tool System

Tools are self-registering modules that declare their own schemas and handlers. This allows:
- Easy addition of new tools
- Dynamic tool availability checking
- Toolset-based enable/disable
- MCP dynamic tool discovery

### Profile Support

Hermes supports multiple fully isolated profiles, each with its own:
- Config directory (`HERMES_HOME`)
- API keys and secrets
- Memory and sessions
- Skills and plugins
- Gateway state

### Prompt Caching

Cache control is applied to system prompts to reduce costs:
- Static prompt sections cached
- Dynamic sections (memory, context) appended fresh
- Cache invalidated only on model/toolset changes

### Context Compression

Automatic context management prevents token limit issues:
- Compresses old messages when approaching limits
- Preserves recent conversation fidelity
- Maintains tool call history

## File Structure

```
hermes-agent/
├── run_agent.py          # AIAgent class
├── model_tools.py        # Tool orchestration
├── toolsets.py           # Toolset definitions
├── cli.py                # CLI orchestrator
├── hermes_state.py       # Session database
├── agent/                # Agent internals
│   ├── prompt_builder.py
│   ├── context_compressor.py
│   ├── prompt_caching.py
│   └── ...
├── hermes_cli/           # CLI subcommands
│   ├── commands.py
│   ├── setup.py
│   ├── skin_engine.py
│   └── ...
├── tools/                # Tool implementations
│   ├── registry.py
│   ├── terminal_tool.py
│   ├── file_tools.py
│   └── ...
├── gateway/              # Messaging gateway
│   ├── run.py
│   ├── session.py
│   └── platforms/
└── environments/         # Terminal backends
    ├── local.py
    ├── docker.py
    └── ...
```

## Related

- [API Reference](../api-reference/overview.md)
- [Adding Tools](../developer-guide/adding-tools.md)
- [Configuration](../user-guide/configuration.md)
