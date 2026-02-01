---
name: python-standards
description: Comprehensive Python coding standards - naming, layout, typing, logging, configuration, and tooling for Python 3.12+
---

# Python Coding Standards

Comprehensive standards for writing clean, maintainable Python code following modern best practices.

## When to Activate

- Starting a new Python project
- Setting up project structure and tooling
- Reviewing code for standards compliance
- Configuring linters and type checkers
- Onboarding new team members

## The Zen of Python

Core principles that guide Python development:

| Principle | Application |
|-----------|-------------|
| Explicit is better than implicit | Don't hide behavior; make intent clear |
| Simple is better than complex | Choose straightforward solutions |
| Readability counts | Code is read more than written |
| Flat is better than nested | Avoid deep nesting; use early returns |
| Errors should never pass silently | Handle or propagate exceptions properly |

```python
# Explicit is better than implicit
# Bad: What does this do?
process(data, True, False, None)

# Good: Named arguments make intent clear
process(data, validate=True, cache=False, timeout=None)
```

## Naming Conventions

### Complete Reference

| Element | Convention | Good Examples | Bad Examples |
|---------|------------|---------------|--------------|
| **Modules** | short, lowercase | `users.py`, `db.py`, `config.py` | `UserManager.py`, `data_base.py` |
| **Packages** | lowercase, no underscores | `mypackage`, `reportgen` | `my_package`, `ReportGen` |
| **Classes** | PascalCase, nouns | `UserProfile`, `ReportGenerator` | `userProfile`, `generate_report` |
| **Functions** | snake_case, verbs | `get_user()`, `calculate_total()` | `GetUser()`, `user()` |
| **Methods** | snake_case, verbs | `process_data()`, `validate()` | `ProcessData()`, `doIt()` |
| **Variables** | snake_case, nouns | `user_list`, `total_count` | `userList`, `x`, `data` |
| **Constants** | UPPER_SNAKE_CASE | `MAX_RETRIES`, `API_TIMEOUT` | `max_retries`, `MaxRetries` |
| **Protected** | _single_underscore | `_internal_cache`, `_validate()` | `internal_cache` |
| **Private** | __double_underscore | `__secret_key` | `_secret_key` |

### Naming Guidelines

```python
# Functions: verb + noun (what it does)
def get_active_users() -> list[User]: ...
def calculate_discount(price: float) -> float: ...
def validate_email(email: str) -> bool: ...

# Variables: descriptive nouns
user_count = len(users)
active_sessions = get_sessions(status="active")
report_config = load_config("report")

# Avoid single letters except in comprehensions
# Bad
for u in users:
    process(u)

# Good
for user in users:
    process(user)

# OK in comprehensions
squares = [x * x for x in numbers]
```

## Code Layout

### Indentation and Spacing

```python
# 4 spaces per indentation level (never tabs)
def process_data(
    data: list[str],
    *,  # Force keyword-only arguments
    timeout: int = 30,
    validate: bool = True,
) -> dict[str, Any]:
    """Process the input data.

    Args:
        data: List of strings to process
        timeout: Operation timeout in seconds
        validate: Whether to validate input

    Returns:
        Processed data as dictionary
    """
    if validate:
        data = [item for item in data if item]

    return {"items": data, "count": len(data)}
```

### Line Length

- **Maximum:** 88 characters (ruff/black default)
- **Docstrings/comments:** 72 characters preferred

```python
# Breaking long lines
result = some_function(
    argument_one,
    argument_two,
    argument_three,
)

# Breaking long strings
message = (
    "This is a very long message that needs to be "
    "split across multiple lines for readability."
)

# Breaking long conditions
if (
    condition_one
    and condition_two
    and condition_three
):
    do_something()
```

### Blank Lines

```python
# Two blank lines before top-level definitions
import os


class UserService:
    """Service for user operations."""

    def __init__(self, db: Database):
        self.db = db

    # One blank line between methods
    def get_user(self, user_id: str) -> User | None:
        return self.db.find_user(user_id)

    def create_user(self, data: UserData) -> User:
        return self.db.create_user(data)


def helper_function():
    """Standalone helper function."""
    pass
```

## Type Hints (Python 3.12+)

### Modern Syntax

```python
# Use built-in generics (not typing.List, typing.Dict)
def process_items(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# Union with | (not typing.Union)
def find_user(user_id: str) -> User | None:
    return users.get(user_id)

# Multiple return types
def parse_value(value: str) -> int | float | str:
    try:
        return int(value)
    except ValueError:
        try:
            return float(value)
        except ValueError:
            return value
```

### When to Use Type Hints

| Scenario | Type Hints Required? |
|----------|---------------------|
| Public function signatures | **Yes** |
| Class attributes | **Yes** |
| Complex variables | **Yes** |
| Simple local variables | No (inferred) |
| Quick scripts | No |
| Tests (assertions are implicit types) | Optional |

### Advanced Typing

```python
from typing import TypeVar, Protocol, Generic
from collections.abc import Callable, Iterator

# TypeVar for generics
T = TypeVar('T')

def first(items: list[T]) -> T | None:
    return items[0] if items else None

# Protocol for duck typing
class Renderable(Protocol):
    def render(self) -> str: ...

def render_all(items: list[Renderable]) -> str:
    return "\n".join(item.render() for item in items)

# Callable types
Handler = Callable[[str, int], bool]

def register_handler(handler: Handler) -> None:
    pass

# Type aliases
JSON = dict[str, "JSON"] | list["JSON"] | str | int | float | bool | None
```

## Imports

### Organization

```python
# Group 1: Standard library
import os
import sys
from pathlib import Path
from typing import Any

# Group 2: Third-party packages
import httpx
from pydantic import BaseModel, Field

# Group 3: Local application
from .models import User, Report
from .utils import get_logger
from . import constants
```

### Best Practices

```python
# Prefer absolute imports
from mypackage.models import User  # Good

# Relative imports OK within package
from .models import User  # OK in same package
from ..utils import helper  # OK for parent package

# Never use star imports
from os import *  # Bad - pollutes namespace

# Import what you need
from os.path import join, exists  # Good

# Or import the module
import os.path  # Also good
os.path.join(...)
```

## Error Handling

### EAFP Pattern

```python
# EAFP: Easier to Ask Forgiveness than Permission
# Preferred in Python

# Good: Try first, handle exception
def get_value(data: dict, key: str, default: Any = None) -> Any:
    try:
        return data[key]
    except KeyError:
        return default

# Avoid: LBYL (Look Before You Leap)
def get_value(data: dict, key: str, default: Any = None) -> Any:
    if key in data:  # Extra lookup
        return data[key]
    return default
```

### Exception Handling

```python
# Catch specific exceptions
try:
    result = process(data)
except ValueError as e:
    logger.warning("Invalid data: %s", e)
    result = default_value
except ConnectionError as e:
    logger.error("Connection failed: %s", e)
    raise

# Chain exceptions to preserve context
try:
    config = load_config(path)
except FileNotFoundError as e:
    raise ConfigError(f"Config not found: {path}") from e
except json.JSONDecodeError as e:
    raise ConfigError(f"Invalid JSON in {path}") from e

# Never use bare except
try:
    risky_operation()
except:  # Bad - catches SystemExit, KeyboardInterrupt
    pass

# If you must catch everything, use Exception
try:
    risky_operation()
except Exception as e:  # Still allows SystemExit, KeyboardInterrupt
    logger.exception("Unexpected error")
    raise
```

### Custom Exception Hierarchy

```python
class AppError(Exception):
    """Base exception for application errors."""

class ValidationError(AppError):
    """Raised when input validation fails."""

class NotFoundError(AppError):
    """Raised when a resource is not found."""

class ConfigError(AppError):
    """Raised when configuration is invalid."""

# Usage
def get_user(user_id: str) -> User:
    user = db.find_user(user_id)
    if not user:
        raise NotFoundError(f"User not found: {user_id}")
    return user
```

## Logging

### Configuration

```python
import logging

# Configure at application startup
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

# Get logger for each module
logger = logging.getLogger(__name__)
```

### Log Levels

| Level | When to Use | Example |
|-------|-------------|---------|
| `DEBUG` | Development details | `logger.debug("Cache hit for key %s", key)` |
| `INFO` | Normal operations | `logger.info("User %s logged in", user_id)` |
| `WARNING` | Recoverable issues | `logger.warning("Retry %d/%d", attempt, max)` |
| `ERROR` | Errors (no traceback) | `logger.error("Failed to connect: %s", err)` |
| `EXCEPTION` | Errors with traceback | `logger.exception("Unexpected error")` |
| `CRITICAL` | System failures | `logger.critical("Database unavailable")` |

### Best Practices

```python
# Use lazy formatting (not f-strings in log calls)
# Good: Only formatted if log level enabled
logger.info("Processing user %s with %d items", user_id, len(items))

# Bad: Always formatted, even if DEBUG disabled
logger.debug(f"Processing user {user_id} with {len(items)} items")

# Include context
logger.info(
    "Request completed",
    extra={"user_id": user_id, "duration_ms": duration}
)

# Use exception() for errors - includes traceback
try:
    process(data)
except Exception:
    logger.exception("Failed to process data")
    raise

# Never use print() for logging
print("User logged in")  # Bad - no levels, no formatting, goes to stdout
logger.info("User logged in")  # Good
```

### Structured Logging

```python
import logging
import json

class JSONFormatter(logging.Formatter):
    def format(self, record: logging.LogRecord) -> str:
        log_data = {
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "logger": record.name,
            "message": record.getMessage(),
        }
        if record.exc_info:
            log_data["exception"] = self.formatException(record.exc_info)
        if hasattr(record, "user_id"):
            log_data["user_id"] = record.user_id
        return json.dumps(log_data)
```

## Configuration Management

### Environment Variables

```python
import os

# Required variables (fail fast if missing)
API_KEY = os.environ["API_KEY"]
DATABASE_URL = os.environ["DATABASE_URL"]

# Optional with defaults
DEBUG = os.getenv("DEBUG", "false").lower() == "true"
LOG_LEVEL = os.getenv("LOG_LEVEL", "INFO")
MAX_CONNECTIONS = int(os.getenv("MAX_CONNECTIONS", "10"))

# Validate at startup
def validate_config():
    """Validate configuration at application startup."""
    if not API_KEY:
        raise ValueError("API_KEY is required")
    if not DATABASE_URL.startswith(("postgres://", "postgresql://")):
        raise ValueError("DATABASE_URL must be a PostgreSQL connection string")
```

### Pydantic Settings

```python
from pydantic_settings import BaseSettings
from pydantic import Field

class Settings(BaseSettings):
    """Application settings loaded from environment."""

    api_key: str = Field(..., description="API key for external service")
    database_url: str = Field(..., description="Database connection URL")
    debug: bool = Field(default=False)
    log_level: str = Field(default="INFO")
    max_connections: int = Field(default=10, ge=1, le=100)

    class Config:
        env_file = ".env"
        env_file_encoding = "utf-8"

# Usage
settings = Settings()
print(settings.api_key)
```

## Project Structure

### Recommended Layout

```
project-root/
├── src/
│   └── mypackage/
│       ├── __init__.py        # Package initialization, version
│       ├── py.typed            # PEP 561 marker for type hints
│       ├── cli.py              # Command-line interface
│       ├── config.py           # Configuration handling
│       ├── constants.py        # Constants and enums
│       ├── models/             # Data models
│       │   ├── __init__.py
│       │   └── user.py
│       ├── services/           # Business logic
│       │   ├── __init__.py
│       │   └── user_service.py
│       └── utils/              # Helper functions
│           ├── __init__.py
│           └── helpers.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py             # pytest fixtures
│   ├── test_models.py
│   └── test_services.py
├── docs/                       # Documentation
├── .env.example                # Environment template
├── .gitignore
├── pyproject.toml              # Project metadata (PEP 621)
└── README.md
```

### pyproject.toml

```toml
[project]
name = "mypackage"
version = "0.1.0"
description = "My Python package"
requires-python = ">=3.12"
dependencies = [
    "httpx>=0.25.0",
    "pydantic>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.4.0",
    "pytest-cov>=4.1.0",
    "ruff>=0.1.0",
    "basedpyright>=1.10.0",
]

[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP"]

[tool.basedpyright]
pythonVersion = "3.12"
typeCheckingMode = "strict"

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-q"
```

## Development Tooling

### UV Commands

| Task | Command |
|------|---------|
| Install package | `uv pip install package-name` |
| Install with dev deps | `uv pip install -e ".[dev]"` |
| Run script | `uv run python script.py` |
| Run tests | `uv run pytest` |
| Run type checker | `uv run basedpyright src tests` |

### Ruff Configuration

```toml
# pyproject.toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = [
    "E",   # pycodestyle errors
    "F",   # pyflakes
    "I",   # isort
    "N",   # pep8-naming
    "W",   # pycodestyle warnings
    "UP",  # pyupgrade
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
]
ignore = [
    "E501",  # line too long (handled by formatter)
]

[tool.ruff.lint.isort]
known-first-party = ["mypackage"]
```

### Pre-commit Hooks

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```

## Quick Reference

### Common Commands

| Task | Command |
|------|---------|
| Format code | `ruff format .` |
| Lint and fix | `ruff check . --fix` |
| Type check | `uv run basedpyright src tests` |
| Run tests | `uv run pytest -q` |
| Run with coverage | `uv run pytest -q --cov=src --cov-report=term-missing` |

### Verification Checklist

- [ ] All public functions have type hints
- [ ] No bare `except:` clauses
- [ ] No `print()` statements (use logging)
- [ ] Proper naming conventions
- [ ] Tests pass: `uv run pytest -q`
- [ ] Formatted: `ruff format .`
- [ ] Linting clean: `ruff check .`
- [ ] Types clean: `uv run basedpyright src tests`
- [ ] Coverage ≥ 80%
