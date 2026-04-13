# Adding Tools

How to create new tools for Hermes Agent.

## Overview

Tools extend Hermes Agent with new capabilities. Each tool is a self-registering module that declares its schema, handler, and requirements.

## Tool Structure

### Basic Tool File

Create `tools/your_tool.py`:

```python
"""Your tool description.

Brief explanation of what the tool does and when to use it.
"""
import json
import os
from typing import Dict, Any
from tools.registry import registry


def check_requirements() -> bool:
    """Check if tool dependencies are available.
    
    Returns:
        True if tool can be used, False otherwise
    """
    return bool(os.getenv("YOUR_API_KEY"))


def your_tool(param: str, task_id: str = None) -> str:
    """Execute the tool.
    
    Args:
        param: Parameter description
        task_id: Task identifier for tracking
        
    Returns:
        JSON string with result
    """
    try:
        # Your implementation here
        result = {"success": True, "data": "result"}
        return json.dumps(result)
    except Exception as e:
        return json.dumps({"success": False, "error": str(e)})


# Register the tool
registry.register(
    name="your_tool",
    toolset="custom",
    schema={
        "name": "your_tool",
        "description": "Clear description of what the tool does",
        "parameters": {
            "type": "object",
            "properties": {
                "param": {
                    "type": "string",
                    "description": "Parameter description"
                }
            },
            "required": ["param"]
        }
    },
    handler=lambda args, **kw: your_tool(
        param=args.get("param", ""),
        task_id=kw.get("task_id")
    ),
    check_fn=check_requirements,
    requires_env=["YOUR_API_KEY"],
    emoji="🔧",
)
```

## Registration Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | `str` | Yes | Unique tool name |
| `toolset` | `str` | Yes | Toolset identifier |
| `schema` | `dict` | Yes | OpenAI-format tool schema |
| `handler` | `Callable` | Yes | Tool execution function |
| `check_fn` | `Callable` | No | Availability check |
| `requires_env` | `list` | No | Required environment variables |
| `is_async` | `bool` | No | Whether handler is async |
| `description` | `str` | No | Tool description |
| `emoji` | `str` | No | Emoji for display |
| `max_result_size_chars` | `int` | No | Max result size |

## Schema Format

Tools use OpenAI-compatible schema format:

```python
schema = {
    "name": "tool_name",
    "description": "What the tool does",
    "parameters": {
        "type": "object",
        "properties": {
            "param1": {
                "type": "string",
                "description": "Description"
            },
            "param2": {
                "type": "integer",
                "description": "Description"
            },
            "optional_param": {
                "type": "string",
                "description": "Description"
            }
        },
        "required": ["param1", "param2"]
    }
}
```

## Handler Requirements

All handlers must:
- Accept `args` dict and `**kwargs`
- Return a JSON string
- Handle errors gracefully
- Use `task_id` from kwargs for tracking

```python
def handler(args: dict, **kwargs) -> str:
    """Tool handler.
    
    Args:
        args: Tool arguments
        **kwargs: Additional context (task_id, etc.)
        
    Returns:
        JSON string result
    """
    task_id = kwargs.get("task_id")
    
    try:
        result = process(args)
        return json.dumps({
            "success": True,
            "data": result,
            "task_id": task_id
        })
    except Exception as e:
        return json.dumps({
            "success": False,
            "error": str(e),
            "task_id": task_id
        })
```

## Tool Discovery

Tools are discovered automatically:

1. Add import in `model_tools.py`:
```python
from tools import your_tool  # Triggers registration
```

2. Add to toolset in `toolsets.py`:
```python
_HERMES_CORE_TOOLS = [
    "terminal",
    "file_read",
    "your_tool",  # Add your tool
]
```

## Example: Web Search Tool

```python
"""Web search tool using Tavily API."""
import json
import os
import requests
from tools.registry import registry


def check_requirements() -> bool:
    """Check if Tavily API key is set."""
    return bool(os.getenv("TAVILY_API_KEY"))


def web_search(query: str, num_results: int = 5) -> str:
    """Search the web.
    
    Args:
        query: Search query
        num_results: Number of results
        
    Returns:
        JSON with search results
    """
    api_key = os.getenv("TAVILY_API_KEY")
    
    response = requests.post(
        "https://api.tavily.com/search",
        json={
            "query": query,
            "api_key": api_key,
            "max_results": num_results
        }
    )
    
    data = response.json()
    
    return json.dumps({
        "success": True,
        "results": [
            {
                "title": r["title"],
                "url": r["url"],
                "content": r["content"]
            }
            for r in data.get("results", [])
        ]
    })


registry.register(
    name="web_search",
    toolset="web",
    schema={
        "name": "web_search",
        "description": "Search the web for current information",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "Search query"
                },
                "num_results": {
                    "type": "integer",
                    "description": "Number of results (default: 5)"
                }
            },
            "required": ["query"]
        }
    },
    handler=lambda args, **kw: web_search(
        query=args.get("query", ""),
        num_results=args.get("num_results", 5)
    ),
    check_fn=check_requirements,
    requires_env=["TAVILY_API_KEY"],
    emoji="🔍",
)
```

## Testing

Write tests for your tool:

```python
# tests/tools/test_your_tool.py
import pytest
from tools.your_tool import your_tool, check_requirements


class TestYourTool:
    """Test your tool."""
    
    def test_check_requirements(self, monkeypatch):
        """Test requirement check."""
        monkeypatch.setenv("YOUR_API_KEY", "test")
        assert check_requirements() is True
    
    def test_your_tool_success(self):
        """Test successful execution."""
        import json
        result = json.loads(your_tool(param="test"))
        assert result["success"] is True
    
    def test_your_tool_error(self):
        """Test error handling."""
        import json
        # Test with invalid input
        result = json.loads(your_tool(param=""))
        assert result["success"] is False or "error" in result
```

## Best Practices

### Error Handling

```python
# Good
def tool_handler(args, **kwargs):
    try:
        result = process(args)
        return json.dumps({"success": True, "data": result})
    except ValueError as e:
        return json.dumps({"success": False, "error": f"Invalid input: {e}"})
    except Exception as e:
        logger.exception("Unexpected error")
        return json.dumps({"success": False, "error": "Internal error"})

# Bad
def tool_handler(args, **kwargs):
    result = process(args)  # Can raise unhandled exceptions
    return str(result)  # Not JSON
```

### Documentation

```python
# Good
def web_search(query: str, num_results: int = 5) -> str:
    """Search the web for current information.
    
    Args:
        query: Search query string
        num_results: Number of results to return (default: 5)
        
    Returns:
        JSON string with search results
        
    Raises:
        requests.RequestException: If API call fails
    """

# Bad
def ws(q, n=5):  # Unclear naming, no docs
    ...
```

### Validation

```python
# Good
def process_file(path: str) -> str:
    if not path:
        return json.dumps({"error": "Path required"})
    if not Path(path).exists():
        return json.dumps({"error": f"File not found: {path}"})
    # Process...

# Bad
def process_file(path):  # No validation
    with open(path) as f:  # Can raise FileNotFoundError
        ...
```

## Related

- [Tool Registry API](../api-reference/tool-registry.md)
- [Contributing](./contributing.md)
