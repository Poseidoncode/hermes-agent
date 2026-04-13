# ToolRegistry Class

Central registry for tool schemas, handlers, and availability checks.

## Overview

The `ToolRegistry` class provides a singleton registry that collects tool schemas and handlers from tool files. It handles schema collection, dispatch, availability checking, and error wrapping.

## Class Definition

```python
class ToolRegistry:
    def __init__(self)
    def register(self, **kwargs)
    def deregister(self, name: str)
    def get_definitions(self, tool_names: Set[str], quiet: bool = False) -> List[dict]
    def dispatch(self, name: str, args: dict, **kwargs) -> str
```

## Methods

### register

Register a tool with the registry. Called at module-import time by each tool file.

```python
def register(
    self,
    name: str,
    toolset: str,
    schema: dict,
    handler: Callable,
    check_fn: Callable = None,
    requires_env: list = None,
    is_async: bool = False,
    description: str = "",
    emoji: str = "",
    max_result_size_chars: int | float | None = None
)
```

**Parameters:**
- `name` (`str`) — Tool name (must be unique)
- `toolset` (`str`) — Toolset identifier
- `schema` (`dict`) — OpenAI-format tool schema
- `handler` (`Callable`) — Tool execution function
- `check_fn` (`Callable`, optional) — Availability check function
- `requires_env` (`list`, optional) — Required environment variables
- `is_async` (`bool`, optional) — Whether handler is async
- `description` (`str`, optional) — Tool description
- `emoji` (`str`, optional) — Emoji for tool display
- `max_result_size_chars` (`int`, optional) — Max result size

**Example:**
```python
from tools.registry import registry

def check_requirements():
    return bool(os.getenv("API_KEY"))

def my_tool(param: str, task_id: str = None):
    return json.dumps({"success": True, "data": "..."})

registry.register(
    name="my_tool",
    toolset="custom",
    schema={
        "name": "my_tool",
        "description": "Does something useful",
        "parameters": {
            "type": "object",
            "properties": {
                "param": {"type": "string"}
            },
            "required": ["param"]
        }
    },
    handler=lambda args, **kw: my_tool(
        param=args.get("param", ""),
        task_id=kw.get("task_id")
    ),
    check_fn=check_requirements,
    requires_env=["API_KEY"],
)
```

### deregister

Remove a tool from the registry.

```python
def deregister(self, name: str) -> None
```

**Parameters:**
- `name` (`str`) — Tool name to remove

**Notes:**
- Also cleans up toolset check if no other tools remain in same toolset
- Used by MCP dynamic tool discovery

### get_definitions

Return OpenAI-format tool schemas for requested tool names.

```python
def get_definitions(
    self,
    tool_names: Set[str],
    quiet: bool = False
) -> List[dict]
```

**Parameters:**
- `tool_names` (`Set[str]`) — Set of tool names to include
- `quiet` (`bool`, optional) — Suppress logging

**Returns:**
- `List[dict]` — Array of tool schemas in OpenAI format

**Example:**
```python
schemas = registry.get_definitions(
    tool_names={"terminal", "file_read", "web_search"}
)
```

### dispatch

Execute a tool handler by name.

```python
def dispatch(self, name: str, args: dict, **kwargs) -> str
```

**Parameters:**
- `name` (`str`) — Tool name to execute
- `args` (`dict`) — Tool arguments
- `**kwargs` — Additional context (task_id, etc.)

**Returns:**
- `str` — JSON string result

**Example:**
```python
result = registry.dispatch(
    name="terminal",
    args={"command": "ls -la"},
    task_id="task_123"
)
data = json.loads(result)
```

## Import Chain

The registry is designed to avoid circular imports:

```
tools/registry.py  (no deps — imported by all tool files)
       ↑
tools/*.py  (each calls registry.register() at import time)
       ↑
model_tools.py  (imports tools/registry + triggers tool discovery)
       ↑
run_agent.py, cli.py, batch_runner.py, environments/
```

## Handler Requirements

All tool handlers MUST:
- Return a JSON string
- Accept `args` dict and `**kwargs`
- Handle errors gracefully
- Use `task_id` from kwargs for tracking

## Related

- [Adding Tools](../developer-guide/adding-tools.md)
- [AIAgent Class](./ai-agent.md)
