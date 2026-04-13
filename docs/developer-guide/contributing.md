# Contributing

Development setup and contribution guidelines for Hermes Agent.

## Development Setup

### Prerequisites

- Python 3.11+
- Git
- uv (package manager)

### Installation

```bash
# Clone repository
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent

# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create virtual environment
uv venv venv --python 3.11
source venv/bin/activate

# Install in development mode
uv pip install -e ".[all,dev]"

# Run tests
python -m pytest tests/ -q
```

### RL Training (Optional)

```bash
# Initialize submodule
git submodule update --init tinker-atropos

# Install
uv pip install -e "./tinker-atropos"
```

## Code Style

### Python Guidelines

- Use type hints for function signatures
- Prefer `pathlib` over `os.path`
- Early returns over nested conditionals
- Docstrings for public APIs
- Logging over print statements

### Example

```python
def process_file(path: Path, timeout: int = 30) -> dict:
    """Process a file and return results.
    
    Args:
        path: File path to process
        timeout: Timeout in seconds
        
    Returns:
        Dict with processing results
        
    Raises:
        FileNotFoundError: If file doesn't exist
    """
    if not path.exists():
        raise FileNotFoundError(f"File not found: {path}")
    
    result = _process_impl(path, timeout)
    if not result:
        return {"success": False}
    
    return {"success": True, "data": result}
```

## Adding Features

### Adding a Tool

1. Create `tools/your_tool.py`:
```python
from tools.registry import registry

def check_requirements() -> bool:
    return bool(os.getenv("API_KEY"))

def your_tool(param: str, task_id: str = None) -> str:
    return json.dumps({"success": True})

registry.register(
    name="your_tool",
    toolset="custom",
    schema={...},
    handler=lambda args, **kw: your_tool(
        param=args.get("param"),
        task_id=kw.get("task_id")
    ),
    check_fn=check_requirements,
)
```

2. Add import in `model_tools.py`
3. Add to `toolsets.py`

### Adding a Slash Command

1. Add to `COMMAND_REGISTRY` in `hermes_cli/commands.py`:
```python
CommandDef(
    "mycommand",
    "Description",
    "Category",
    aliases=("mc",),
    args_hint="[arg]"
)
```

2. Add handler in `cli.py`:
```python
elif canonical == "mycommand":
    self._handle_mycommand(cmd_original)
```

3. Add gateway handler if needed in `gateway/run.py`

### Adding a Platform

1. Create `gateway/platforms/your_platform.py`
2. Inherit from `BasePlatform`
3. Implement required methods
4. Add to `gateway/run.py` platform list

See `gateway/platforms/ADDING_A_PLATFORM.md` for details.

## Testing

### Running Tests

```bash
# All tests
python -m pytest tests/ -q

# Specific module
python -m pytest tests/test_tools.py -v

# With coverage
python -m pytest tests/ --cov=hermes_cli --cov-report=html
```

### Writing Tests

```python
def test_your_feature():
    """Test description."""
    # Arrange
    agent = AIAgent(quiet_mode=True)
    
    # Act
    response = agent.chat("test message")
    
    # Assert
    assert response is not None
    assert "expected" in response
```

## Pull Request Process

1. **Create branch:**
```bash
git checkout -b feature/your-feature
```

2. **Make changes** following code style

3. **Run tests:**
```bash
python -m pytest tests/ -q
```

4. **Commit changes:**
```bash
git add .
git commit -m "feat: add your feature"
```

5. **Push and create PR:**
```bash
git push origin feature/your-feature
```

### Commit Message Format

Follow Conventional Commits:

- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `style:` Formatting
- `refactor:` Code restructuring
- `test:` Tests
- `chore:` Maintenance

## Documentation

### Updating Docs

1. Edit files in `docs/` directory
2. Follow documentation style guide
3. Include code examples
4. Update related pages

### Documentation Style

- Clear, concise language
- Code examples for all APIs
- Screenshots for UI features
- Troubleshooting sections

## Code Review

### Checklist

- [ ] Tests pass
- [ ] Type hints added
- [ ] Docstrings complete
- [ ] No debug code
- [ ] Error handling added
- [ ] Logging appropriate
- [ ] Documentation updated

### Review Process

1. PR created
2. Automated checks run
3. Maintainer review
4. Address feedback
5. Merge approval

## Release Process

### Version Bump

1. Update version in `pyproject.toml`
2. Update `CHANGELOG.md`
3. Create release branch
4. Test thoroughly
5. Tag and release

### Release Checklist

- [ ] All tests pass
- [ ] Documentation updated
- [ ] Changelog complete
- [ ] Version bumped
- [ ] Tag created
- [ ] Release notes written

## Getting Help

- **Discord:** https://discord.gg/NousResearch
- **Issues:** https://github.com/NousResearch/hermes-agent/issues
- **Discussions:** https://github.com/NousResearch/hermes-agent/discussions

## Related

- [Code Style](./code-style.md)
- [Adding Tools](./adding-tools.md)
- [Testing](./testing.md)
