# Project: bpc-dot-github

**Last Updated:** 2026-02-01

## Overview

Organization profile repository (`.github`) for BusinessPlus Community. Contains community health files, GitHub templates, and shared Claude configuration that applies across all organization repositories.

## Purpose

- **Organization profile** - README displayed on GitHub org page
- **Community health files** - CODE_OF_CONDUCT, CONTRIBUTING, SECURITY, SUPPORT
- **GitHub templates** - Issue templates, PR template, FUNDING.yml
- **Claude configuration** - Rules and skills shared across org repos

## Directory Structure

```
.claude/           # Claude Code configuration (rules, skills, commands)
.github/           # GitHub templates (issues, PRs, funding)
docs/              # Member documentation and plans
profile/           # Organization profile README
context/           # Reference materials (screenshots, docs)
```

## Key Files

- `README.md` / `profile/README.md` - Organization profile (kept in sync)
- `CODE_OF_CONDUCT.md` - Contributor Covenant 3.0
- `CONTRIBUTING.md` - Contribution guidelines
- `SECURITY.md` - Vulnerability reporting policy
- `SUPPORT.md` - Support channels

## Repository Type

This is a **documentation-only** repository with no application code. Changes are primarily Markdown edits to community files and templates.

## Notes

- Claude configuration here is shared across BusinessPlus Community repos
- Python rules/skills are included for use in code repositories like bpc-pycdd
- Always keep README.md and profile/README.md in sync
