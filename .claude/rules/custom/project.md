# Project: bpc-pycdd

**Last Updated:** 2026-02-01

## Overview

Python library providing helper functions for CDD report generation. Target users are CDD administrators (not Python developers). Currently a skeleton awaiting vendor Python API specifications.

## Technology Stack

- **Language:** Python 3.12
- **Package Manager:** uv
- **Type Checker:** basedpyright (strict mode)
- **Linter/Formatter:** ruff
- **Testing:** pytest with pytest-cov

## Directory Structure

```
src/bpc_pycdd/     # Source code (typed)
tests/             # Test files mirror source structure
```

## Development Commands

```bash
# Install
uv pip install -e ".[dev]"

# Tests
uv run pytest -q
uv run pytest -q --cov=bpc_pycdd --cov-report=term-missing

# Quality
ruff format .
ruff check . --fix
uv run basedpyright src tests
```

## Project-Specific Notes

- **TDD mandatory** - Write failing test first, then implement
- **Coverage target** - 90% minimum (configured in pyproject.toml)
- **Audience** - CDD administrators, prioritize usability over cleverness
- **Vendor dependency** - Architecture decisions await vendor Python API specs
