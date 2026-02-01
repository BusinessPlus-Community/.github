## Python Coding Standards

**Tooling:** uv (package management) | ruff (format + lint) | basedpyright (types) | pytest (tests)

### Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Modules | short, lowercase | `users.py`, `db.py` |
| Packages | lowercase, no underscores | `mypackage` |
| Classes | PascalCase, nouns | `UserProfile`, `ReportGenerator` |
| Functions | snake_case, verbs | `get_user()`, `generate_report()` |
| Variables | snake_case, nouns | `user_list`, `report_config` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRIES`, `DEFAULT_TIMEOUT` |
| Protected | single underscore prefix | `_internal_method()` |
| Private | double underscore prefix | `__very_private` |

### Code Layout

- **Indentation:** 4 spaces (no tabs)
- **Line length:** 88 characters (ruff/black default)
- **Blank lines:** 2 between top-level definitions, 1 between methods
- **Trailing whitespace:** None
- **File ending:** Single newline

### Type Hints (Python 3.12+)

```python
# Use built-in generics (not typing.List, typing.Dict)
def process(items: list[str]) -> dict[str, int]: ...

# Union with | (not typing.Union or typing.Optional)
def find(id: str) -> User | None: ...

# Required on: public function signatures, complex variables
# Optional for: simple scripts, obvious types
```

### Imports

```python
# Order: stdlib → third-party → local
import os
from pathlib import Path

import httpx
from pydantic import BaseModel

from .models import User
from .utils import get_logger
```

Ruff auto-sorts: `ruff check . --fix`

### Error Handling

```python
# EAFP: Try first, handle exception
try:
    value = data[key]
except KeyError:
    value = default

# Chain exceptions to preserve context
except ValueError as e:
    raise ConfigError(f"Invalid config: {path}") from e

# Never bare except - catch specific exceptions
```

### Logging

```python
import logging

logger = logging.getLogger(__name__)

# Use appropriate levels
logger.debug("Processing item %s", item_id)    # Development details
logger.info("User %s logged in", user_id)       # Normal operations
logger.warning("Retry %d of %d", attempt, max)  # Recoverable issues
logger.error("Failed to connect: %s", err)      # Errors
logger.exception("Unexpected error")            # Errors with traceback
```

**Never use `print()` for logging in production code.**

### Configuration

```python
# Use environment variables
import os
API_KEY = os.environ["API_KEY"]  # Required (raises if missing)
DEBUG = os.getenv("DEBUG", "false").lower() == "true"  # Optional with default

# Validate at startup, fail fast
if not API_KEY:
    raise ValueError("API_KEY environment variable required")
```

### Package Management (UV ONLY)

**MANDATORY: Use `uv` for ALL Python operations. Never use `pip` directly.**

| Task | Command |
|------|---------|
| Install package | `uv pip install package-name` |
| Install dev deps | `uv pip install -e ".[dev]"` |
| Run script | `uv run python script.py` |
| Run tests | `uv run pytest` |
| Run type checker | `uv run basedpyright src` |

### Quality Commands

| Task | Command |
|------|---------|
| Format | `ruff format .` |
| Lint + fix | `ruff check . --fix` |
| Type check | `uv run basedpyright src tests` |
| Tests | `uv run pytest -q` |
| Coverage | `uv run pytest -q --cov=src --cov-report=term-missing` |

### Verification Checklist

Before completing Python work:

- [ ] Used `uv` for all package operations
- [ ] Tests pass: `uv run pytest -q`
- [ ] Code formatted: `ruff format .`
- [ ] Linting clean: `ruff check .`
- [ ] Type checking: `uv run basedpyright src tests`
- [ ] Coverage ≥ 80%
- [ ] No `print()` statements (use logging)
- [ ] No bare `except:` clauses
- [ ] Proper naming conventions followed
