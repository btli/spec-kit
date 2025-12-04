# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Spec Kit is a toolkit for Spec-Driven Development (SDD) - a methodology where specifications generate code rather than just guiding it. The project consists of:

1. **Specify CLI** (`src/specify_cli/__init__.py`) - Python CLI tool that bootstraps projects with templates and AI agent integrations
2. **Templates** (`templates/`) - Markdown templates for specs, plans, tasks, and slash commands
3. **Scripts** (`scripts/bash/`, `scripts/powershell/`) - Shell scripts for feature creation and agent context updates
4. **Documentation** - SDD methodology guides and contributor docs

## Development Commands

```bash
# Install dependencies
uv sync

# Run CLI directly (fastest for iteration)
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools

# Install in editable mode
uv pip install -e .
specify --help

# Run via uvx from repo root
uvx --from . specify init demo --ai copilot --ignore-agent-tools

# Build wheel
uv build
```

## Architecture

### CLI Structure

The entire CLI is in a single file (`src/specify_cli/__init__.py`) using Typer/Rich. Key components:

- **AGENT_CONFIG dict** - Single source of truth for all supported AI agents (name, folder, install_url, requires_cli)
- **Commands**: `init` (bootstrap project), `check` (verify tool installation)
- Downloads release packages from GitHub, extracts templates, sets up agent-specific command directories

### Agent Integration

Each AI agent has different conventions:
- **Command directories**: `.claude/commands/`, `.gemini/commands/`, `.github/agents/`, etc.
- **File formats**: Markdown (most agents) or TOML (Gemini, Qwen)
- **Argument placeholders**: `$ARGUMENTS` (Markdown), `{{args}}` (TOML)

### Template System

Templates use placeholder patterns:
- `{SCRIPT}` - Replaced with actual script path
- `__AGENT__` - Replaced with agent name
- `[NEEDS CLARIFICATION: ...]` markers for ambiguities

### Script Variants

All scripts have both Bash (`.sh`) and PowerShell (`.ps1`) versions. The CLI auto-selects based on OS or accepts `--script sh|ps`.

## Key Conventions

### Adding New AI Agents

See `AGENTS.md` for complete guide. Critical rule: **Use actual CLI tool name as AGENT_CONFIG key** (e.g., `"cursor-agent"` not `"cursor"`) to avoid special-case mappings.

### Version Changes

Any CLI changes require:
1. Version bump in `pyproject.toml`
2. Entry in `CHANGELOG.md`

### Testing Template Changes Locally

```bash
# Generate release packages locally
./.github/workflows/scripts/create-release-packages.sh v1.0.0

# Copy to test project
cp -r .genreleases/sdd-copilot-package-sh/. /path/to/test-project/
```

## Slash Commands (After specify init)

Projects initialized with Spec Kit get these slash commands:

| Command | Purpose |
|---------|---------|
| `/speckit.constitution` | Create project governing principles |
| `/speckit.specify` | Define requirements and user stories |
| `/speckit.plan` | Create technical implementation plan |
| `/speckit.tasks` | Generate actionable task list |
| `/speckit.implement` | Execute tasks to build feature |
| `/speckit.clarify` | Clarify underspecified areas |
| `/speckit.analyze` | Cross-artifact consistency check |
| `/speckit.checklist` | Generate quality checklists |

## SDD Workflow

1. `specify init <project>` - Bootstrap project with templates
2. `/speckit.constitution` - Establish principles
3. `/speckit.specify` - Define WHAT/WHY (no tech stack)
4. `/speckit.clarify` - Resolve ambiguities
5. `/speckit.plan` - Define HOW (tech stack, architecture)
6. `/speckit.tasks` - Break into actionable tasks
7. `/speckit.implement` - Execute implementation

## File Locations

- `memory/constitution.md` - Project principles (template)
- `specs/<feature>/spec.md` - Feature specification
- `specs/<feature>/plan.md` - Implementation plan
- `specs/<feature>/tasks.md` - Task breakdown
- `specs/<feature>/contracts/` - API specs, data models
