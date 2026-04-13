# Building Your First Skill

Create a custom skill for Hermes Agent.

## Overview

Skills extend Hermes Agent with custom capabilities. This tutorial walks through creating a simple skill from scratch.

## Prerequisites

- Hermes Agent installed
- Basic Python knowledge
- Text editor

## Step 1: Create Skill Directory

```bash
# Navigate to skills directory
cd ~/.hermes/skills/

# Create skill directory
mkdir -p my-first-skill
cd my-first-skill
```

## Step 2: Create SKILL.md

The `SKILL.md` file defines the skill's purpose and instructions:

```markdown
# My First Skill

A simple skill that demonstrates the basics.

## Purpose

This skill helps you learn the skill system by creating a simple example.

## Instructions

When the user asks about skills:
1. Explain what skills are
2. Show how to create them
3. Provide examples

## Commands

- `/my-skill help` — Show help message
- `/my-skill demo` — Run a demo
```

## Step 3: Create Skill Handler (Optional)

For more complex skills, create a Python handler:

```python
# handler.py
import json
from typing import Dict, Any

def handle_my_skill(action: str, **kwargs) -> str:
    """Handle skill actions.
    
    Args:
        action: Action to perform
        **kwargs: Additional arguments
        
    Returns:
        JSON response string
    """
    if action == "help":
        return json.dumps({
            "success": True,
            "message": "My Skill Help:\n- /my-skill help\n- /my-skill demo"
        })
    elif action == "demo":
        return json.dumps({
            "success": True,
            "message": "Running demo..."
        })
    else:
        return json.dumps({
            "success": False,
            "error": f"Unknown action: {action}"
        })
```

## Step 4: Register the Skill

Skills are automatically discovered. Place in `~/.hermes/skills/`:

```
~/.hermes/skills/
└── my-first-skill/
    ├── SKILL.md
    └── handler.py (optional)
```

## Step 5: Test the Skill

```bash
# Start Hermes
hermes

# Use the skill
/my-first-skill help
/my-first-skill demo
```

## Step 6: Add to Skills Hub (Optional)

To share with the community:

1. Create a GitHub repository
2. Add README with documentation
3. Submit to agentskills.io

## Example: Weather Skill

Here's a more complete example:

```markdown
# Weather Skill

Get current weather and forecasts.

## Purpose

Provides weather information for any location.

## Instructions

When user asks about weather:
1. Extract location from query
2. Call weather API
3. Format and present results

## Commands

- `/weather [location]` — Current weather
- `/forecast [location] [days]` — Forecast
```

```python
# handler.py
import os
import requests
import json

def get_weather(location: str) -> str:
    """Get current weather."""
    api_key = os.getenv("WEATHER_API_KEY")
    
    response = requests.get(
        "https://api.weather.com/current",
        params={
            "location": location,
            "key": api_key
        }
    )
    
    data = response.json()
    
    return json.dumps({
        "success": True,
        "temperature": data["temp"],
        "conditions": data["conditions"],
        "location": location
    })

def get_forecast(location: str, days: int = 3) -> str:
    """Get weather forecast."""
    # Similar implementation
    pass
```

## Best Practices

### Skill Structure

- Keep skills focused on single purpose
- Use clear, descriptive names
- Document all commands
- Handle errors gracefully

### Code Quality

```python
# Good
def get_weather(location: str) -> str:
    """Get current weather for location.
    
    Args:
        location: City or coordinates
        
    Returns:
        JSON with weather data
    """
    if not location:
        return json.dumps({"error": "Location required"})
    
    # Implementation

# Bad
def gw(l):  # Unclear naming, no docs
    if not l:
        return "error"
```

### Error Handling

```python
def handle_request(action: str, **kwargs) -> str:
    try:
        result = _process(action, **kwargs)
        return json.dumps({"success": True, "data": result})
    except ValueError as e:
        return json.dumps({"success": False, "error": str(e)})
    except Exception as e:
        logger.exception("Unexpected error")
        return json.dumps({"success": False, "error": "Internal error"})
```

## Testing

Test your skill thoroughly:

```bash
# Test all commands
/my-skill help
/my-skill demo
/my-skill test-case-1
/my-skill test-case-2

# Test error cases
/my-skill invalid-command
/my-skill --missing-args
```

## Sharing

### Publish to Skills Hub

1. Create GitHub repo
2. Add comprehensive README
3. Include examples
4. Submit to agentskills.io

### Documentation

Your README should include:
- Skill description
- Installation instructions
- Usage examples
- Configuration options
- API reference

## Next Steps

- Explore existing skills in `~/.hermes/skills/`
- Check out advanced skills on agentskills.io
- Join Discord for community support
- Contribute to Skills Hub

## Related

- [Skills System](../features/skills.md)
- [Skills Hub](../features/skills-hub.md)
