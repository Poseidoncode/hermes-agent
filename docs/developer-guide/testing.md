# Testing

Test suite organization and testing procedures for Hermes Agent.

## Overview

Hermes Agent has a comprehensive test suite with ~3000 tests covering:
- Unit tests for individual modules
- Integration tests for component interactions
- End-to-end tests for full workflows
- Performance benchmarks

## Running Tests

### Basic Test Run

```bash
# All tests
python -m pytest tests/ -q

# Verbose output
python -m pytest tests/ -v

# With coverage
python -m pytest tests/ --cov=hermes_cli --cov-report=html
```

### Test Categories

```bash
# Unit tests only
python -m pytest tests/unit/ -q

# Integration tests
python -m pytest tests/integration/ -q

# E2E tests
python -m pytest tests/e2e/ -q

# Specific module
python -m pytest tests/test_tools.py -v
```

### Parallel Execution

```bash
# Run tests in parallel
python -m pytest tests/ -n auto

# With specific worker count
python -m pytest tests/ -n 4
```

## Test Organization

### Directory Structure

```
tests/
├── unit/                 # Unit tests
│   ├── test_tools.py
│   ├── test_registry.py
│   └── test_prompt_builder.py
├── integration/          # Integration tests
│   ├── test_agent_loop.py
│   └── test_gateway.py
├── e2e/                 # End-to-end tests
│   └── test_conversation.py
├── cron/                # Cron tests
│   ├── test_jobs.py
│   └── test_scheduler.py
├── tools/               # Tool tests
│   ├── test_terminal.py
│   ├── test_files.py
│   └── test_web.py
└── gateway/             # Gateway tests
    ├── test_session.py
    └── test_platforms.py
```

## Writing Tests

### Test Structure

```python
"""Test module description."""
import pytest
from run_agent import AIAgent
from tools.registry import registry


class TestAIAgent:
    """Test AIAgent class."""
    
    def test_chat_basic(self):
        """Test basic chat functionality."""
        # Arrange
        agent = AIAgent(quiet_mode=True)
        
        # Act
        response = agent.chat("Hello")
        
        # Assert
        assert response is not None
        assert len(response) > 0
    
    def test_run_conversation(self):
        """Test full conversation flow."""
        # Arrange
        agent = AIAgent(max_iterations=5)
        
        # Act
        result = agent.run_conversation(
            user_message="Test message",
            conversation_history=[]
        )
        
        # Assert
        assert "final_response" in result
        assert "messages" in result
```

### Fixtures

```python
# conftest.py
import pytest
from pathlib import Path


@pytest.fixture
def temp_hermes_home(tmp_path):
    """Create temporary HERMES_HOME directory."""
    hermes_home = tmp_path / ".hermes"
    hermes_home.mkdir()
    return hermes_home


@pytest.fixture
def agent(temp_hermes_home):
    """Create test agent instance."""
    return AIAgent(
        quiet_mode=True,
        skip_memory=True,
        skip_context_files=True
    )


@pytest.fixture
def sample_conversation():
    """Sample conversation history."""
    return [
        {"role": "user", "content": "Hello"},
        {"role": "assistant", "content": "Hi there!"}
    ]
```

### Parametrized Tests

```python
import pytest


@pytest.mark.parametrize("model,expected", [
    ("anthropic/claude-3-5-sonnet", True),
    ("openai/gpt-4", True),
    ("invalid/model", False),
])
def test_model_validation(model, expected):
    """Test model validation."""
    result = validate_model(model)
    assert result == expected
```

### Async Tests

```python
import pytest
import asyncio


@pytest.mark.asyncio
async def test_async_tool():
    """Test async tool execution."""
    result = await async_tool(param="value")
    assert result is not None
```

## Test Coverage

### Coverage Report

```bash
# Generate HTML report
python -m pytest tests/ --cov=hermes_cli --cov-report=html

# View in browser
open htmlcov/index.html

# Terminal report
python -m pytest tests/ --cov=hermes_cli --cov-report=term-missing
```

### Coverage Requirements

| Component | Minimum Coverage |
|-----------|-----------------|
| Core modules | 80% |
| Tools | 75% |
| Gateway | 70% |
| CLI | 75% |

## Mocking and Fixtures

### Mocking External Services

```python
from unittest.mock import Mock, patch
import pytest


@patch("tools.web_tools.requests.get")
def test_web_search(mock_get):
    """Test web search with mocked HTTP."""
    # Arrange
    mock_response = Mock()
    mock_response.json.return_value = {"results": []}
    mock_get.return_value = mock_response
    
    # Act
    result = web_search("query")
    
    # Assert
    assert result is not None
    mock_get.assert_called_once()
```

### Temporary Files

```python
def test_file_operations(tmp_path):
    """Test file operations with temp directory."""
    # Arrange
    test_file = tmp_path / "test.txt"
    test_file.write_text("content")
    
    # Act
    result = read_file(test_file)
    
    # Assert
    assert result == "content"
```

### Environment Variables

```python
import os
import pytest


def test_env_required(monkeypatch):
    """Test environment variable handling."""
    # Arrange
    monkeypatch.setenv("API_KEY", "test-key")
    
    # Act
    result = check_requirements()
    
    # Assert
    assert result is True
```

## Performance Testing

### Benchmark Tests

```python
import pytest
import time


def test_response_time(agent):
    """Test agent response time."""
    start = time.time()
    agent.chat("Quick question")
    elapsed = time.time() - start
    
    # Should respond within 30 seconds
    assert elapsed < 30
```

### Load Testing

```python
def test_concurrent_requests():
    """Test concurrent request handling."""
    import concurrent.futures
    
    def make_request():
        agent = AIAgent(quiet_mode=True)
        return agent.chat("Test")
    
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        futures = [executor.submit(make_request) for _ in range(10)]
        results = [f.result() for f in futures]
    
    assert all(results)
```

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/tests.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.11", "3.12"]
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Install uv
        run: curl -LsSf https://astral.sh/uv/install.sh | sh
      
      - name: Install dependencies
        run: |
          uv venv venv --python ${{ matrix.python-version }}
          source venv/bin/activate
          uv pip install -e ".[all,dev]"
      
      - name: Run tests
        run: |
          source venv/bin/activate
          python -m pytest tests/ -q --cov=hermes_cli
```

## Debugging Tests

### Verbose Output

```bash
# Show print statements
python -m pytest tests/ -s

# Show local variables on failure
python -m pytest tests/ -l

# Stop on first failure
python -m pytest tests/ -x
```

### Debugging Failed Tests

```bash
# Run specific test
python -m pytest tests/test_tools.py::test_specific_tool -v

# With pdb debugger
python -m pytest tests/test_tools.py --pdb

# Log output
python -m pytest tests/ --log-cli-level=DEBUG
```

## Best Practices

### Test Naming

```python
# Good
def test_chat_returns_string():
def test_tool_raises_on_invalid_input():
def test_memory_persists_across_sessions():

# Bad
def test1():
def test_stuff():
```

### Assertions

```python
# Good
assert result == expected
assert len(items) > 0
assert "key" in response

# Bad
assert result  # Unclear what's being tested
```

### Test Independence

```python
# Good - each test is independent
def test_one():
    setup_fresh_state()
    # test logic

def test_two():
    setup_fresh_state()
    # test logic

# Bad - tests depend on order
def test_one():
    global_state = "modified"

def test_two():
    # Depends on test_one running first
    assert global_state == "modified"
```

## Related

- [Contributing](./contributing.md)
- [Code Style](./code-style.md)
