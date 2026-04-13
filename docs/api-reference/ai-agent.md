# AIAgent Class

Core agent class for managing conversations with tool calling capabilities.

## Overview

The `AIAgent` class provides a clean, standalone agent that executes AI models with tool calling capabilities. It handles the conversation loop, tool execution, and response management.

## Class Definition

```python
class AIAgent:
    def __init__(
        self,
        model: str = "anthropic/claude-opus-4.6",
        max_iterations: int = 90,
        enabled_toolsets: list = None,
        disabled_toolsets: list = None,
        quiet_mode: bool = False,
        save_trajectories: bool = False,
        platform: str = None,
        session_id: str = None,
        skip_context_files: bool = False,
        skip_memory: bool = False,
        **kwargs
    )
```

## Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `model` | `str` | `"anthropic/claude-opus-4.6"` | Model provider and model name |
| `max_iterations` | `int` | `90` | Maximum tool calling iterations |
| `enabled_toolsets` | `list` | `None` | Toolsets to enable (None = all) |
| `disabled_toolsets` | `list` | `None` | Toolsets to disable |
| `quiet_mode` | `bool` | `False` | Suppress tool output display |
| `save_trajectories` | `bool` | `False` | Save conversation trajectories |
| `platform` | `str` | `None` | Platform identifier (cli, telegram, etc.) |
| `session_id` | `str` | `None` | Session identifier for persistence |
| `skip_context_files` | `bool` | `False` | Skip loading context files |
| `skip_memory` | `bool` | `False` | Skip memory loading |

## Methods

### chat

Simple interface that returns the final response string.

```python
def chat(self, message: str) -> str
```

**Parameters:**
- `message` (`str`) — User message to process

**Returns:**
- `str` — Agent's final response

**Example:**
```python
agent = AIAgent()
response = agent.chat("What's the weather today?")
print(response)
```

### run_conversation

Full interface that returns a dict with response and message history.

```python
def run_conversation(
    self,
    user_message: str,
    system_message: str = None,
    conversation_history: list = None,
    task_id: str = None
) -> dict
```

**Parameters:**
- `user_message` (`str`) — User message to process
- `system_message` (`str`, optional) — Custom system prompt
- `conversation_history` (`list`, optional) — Previous messages
- `task_id` (`str`, optional) — Task identifier for tracking

**Returns:**
- `dict` — Contains `final_response` and `messages` array

**Example:**
```python
result = agent.run_conversation(
    user_message="Analyze this code",
    conversation_history=[previous_messages]
)
print(result["final_response"])
```

## Agent Loop

The core conversation loop runs synchronously:

```python
while api_call_count < self.max_iterations and self.iteration_budget.remaining > 0:
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        tools=tool_schemas
    )
    
    if response.tool_calls:
        for tool_call in response.tool_calls:
            result = handle_function_call(
                tool_call.name,
                tool_call.args,
                task_id
            )
            messages.append(tool_result_message(result))
        api_call_count += 1
    else:
        return response.content
```

## Message Format

Messages follow OpenAI format:

```python
{"role": "system/user/assistant/tool", "content": "..."}
```

Reasoning content is stored in `assistant_msg["reasoning"]`.

## Related

- [Tool Registry](./tool-registry.md)
- [Architecture Overview](../architecture/overview.md)
