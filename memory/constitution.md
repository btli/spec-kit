# [PROJECT_NAME] Constitution
<!-- Example: Spec Constitution, TaskFlow Constitution, etc. -->

## Core Principles

### [PRINCIPLE_1_NAME]
<!-- Example: I. Library-First -->
[PRINCIPLE_1_DESCRIPTION]
<!-- Example: Every feature starts as a standalone library; Libraries must be self-contained, independently testable, documented; Clear purpose required - no organizational-only libraries -->

### [PRINCIPLE_2_NAME]
<!-- Example: II. CLI Interface -->
[PRINCIPLE_2_DESCRIPTION]
<!-- Example: Every library exposes functionality via CLI; Text in/out protocol: stdin/args → stdout, errors → stderr; Support JSON + human-readable formats -->

### [PRINCIPLE_3_NAME]
<!-- Example: III. Test-First (NON-NEGOTIABLE) -->
[PRINCIPLE_3_DESCRIPTION]
<!-- Example: TDD mandatory: Tests written → User approved → Tests fail → Then implement; Red-Green-Refactor cycle strictly enforced -->

### [PRINCIPLE_4_NAME]
<!-- Example: IV. Integration Testing -->
[PRINCIPLE_4_DESCRIPTION]
<!-- Example: Focus areas requiring integration tests: New library contract tests, Contract changes, Inter-service communication, Shared schemas -->

### [PRINCIPLE_5_NAME]
<!-- Example: V. Observability, VI. Versioning & Breaking Changes, VII. Simplicity -->
[PRINCIPLE_5_DESCRIPTION]
<!-- Example: Text I/O ensures debuggability; Structured logging required; Or: MAJOR.MINOR.BUILD format; Or: Start simple, YAGNI principles -->

## Secrets Management (NON-NEGOTIABLE)

All secrets MUST be managed through Phase (phase.dev). This is NON-NEGOTIABLE.

### Requirements

- **No hardcoded secrets**: API keys, tokens, credentials, and sensitive configuration MUST NOT be committed to the repository
- **No local .env files in production**: Development may use .env files (gitignored), but production/staging MUST use Phase
- **Phase CLI integration**: Use `phase run` to inject secrets at runtime or `phase secrets get` for retrieval
- **Environment separation**: Secrets MUST be organized by environment (development, staging, production)

### Implementation

- Use the `phase-secrets` skill for secrets operations
- Application: `claude-code` (or project-specific app name)
- Commands:
  - `phase secrets list --app APP --env ENV`
  - `phase secrets get SECRET_NAME --app APP --env ENV`
  - `phase run --app APP --env ENV -- command`

### Violations

Any code that:
- Hardcodes secrets or credentials
- Commits .env files with real values
- Bypasses Phase for production secrets

Is a CRITICAL constitution violation and MUST be remediated before merge.

## Project Management (NON-NEGOTIABLE)

All work MUST be tracked in Plane (plane.joyful.house). This is NON-NEGOTIABLE.

### Requirements

- **Issue tracking**: Every task from tasks.md MUST have a corresponding Plane issue
- **Status updates**: Issue status MUST be updated as work progresses (Todo → In Progress → Done)
- **Traceability**: Commits SHOULD reference Plane issue IDs
- **Sprint alignment**: Tasks MUST be assigned to appropriate cycles/sprints

### Implementation

- Use the `plane-project-management` skill for Plane operations
- Workspace: `kaelyn-ai`
- API authentication via Phase: `phase secrets get PLANE_API_KEY`

### Workflow Integration

- `/speckit.plan`: Create epic/parent issue for feature
- `/speckit.tasks`: Create child issues for each task, linked to parent
- `/speckit.implement`: Update issue status as tasks complete
- `/speckit.analyze`: Verify Plane issue coverage

### Violations

Any implementation that:
- Creates tasks without corresponding Plane issues
- Completes work without updating issue status
- Merges without issue traceability

Is a constitution violation and MUST be remediated.

## Package Managers (NON-NEGOTIABLE)

Use modern, fast package managers. This is NON-NEGOTIABLE.

### Python Projects

- **ALWAYS use `uv`** for all Python operations
- **NEVER use** `pip`, `pip3`, `python -m pip`, or `python3 -m pip`
- **Python 3.13+** unless project explicitly requires older version
- Commands:
  - `uv sync` - Install dependencies
  - `uv run pytest` - Run tests
  - `uv run ruff check --fix && uv run ruff format` - Lint and format
  - `uv add <package>` - Add dependency
  - `uv build` - Build package

### JavaScript/TypeScript Projects

- **ALWAYS use `bun`** for all JS/TS operations
- **NEVER use** `npm`, `yarn`, or `pnpm`
- Commands:
  - `bun install` - Install dependencies
  - `bun run <script>` - Run scripts
  - `bun test` - Run tests
  - `bun add <package>` - Add dependency

### Violations

Using deprecated package managers (`pip`, `npm`, `yarn`) is a constitution violation.

## Dependency Versions (NON-NEGOTIABLE)

Always use latest stable versions. This is NON-NEGOTIABLE.

### Before Adding/Updating Dependencies

1. **Check latest stable version** from official source:
   - Python: Fetch `https://pypi.org/project/{package}/`
   - JS/TS: Fetch `https://www.npmjs.com/package/{package}`
2. **Use latest stable version** unless documented compatibility issue
3. **Document rationale** if pinning to non-latest version

### Version Verification

When installing packages, verify you're getting the latest:
```bash
# Python - check PyPI
curl -s https://pypi.org/pypi/{package}/json | jq -r '.info.version'

# JS/TS - check npm
bun info {package} version
```

## Code Quality (NON-NEGOTIABLE)

Maintain strict code quality standards. This is NON-NEGOTIABLE.

### Linter Rules

- **NEVER disable linter rules** using:
  - `eslint-disable`, `@ts-ignore`, `@ts-expect-error` (JS/TS)
  - `# noqa`, `# type: ignore`, `# pylint: disable` (Python)
- **Fix root cause** by refactoring code to comply
- If rule is overly restrictive, propose project-wide configuration change

### Pre-Commit Validation

Run before every commit:

**Python:**
```bash
uv run ruff check --fix && uv run ruff format && uv run mypy --strict . && uv run pytest
```

**JavaScript/TypeScript:**
```bash
bun run lint && bun run typecheck && bun run test
```

### Violations

- Disabling linter rules locally
- Committing without passing lint/type/test checks
- Ignoring type errors instead of fixing them

## Docker Images (NON-NEGOTIABLE)

Use auto-updating tags for maintainability. This is NON-NEGOTIABLE.

### Required Tag Patterns

| Runtime | Correct | Incorrect |
|---------|---------|-----------|
| Bun | `oven/bun:alpine` | `oven/bun:1.2.14-alpine` |
| Python | `python:3-slim` | `python:3.12-slim` |
| Redis | `redis:alpine` | `redis:8.2.2-alpine` |
| Node | `node:lts-alpine` | `node:22-alpine` |

### Rationale

- Reduces maintenance burden
- Ensures automatic security updates
- Prevents version drift across environments

## Asyncio Best Practices (Python)

Follow modern async patterns. These are NON-NEGOTIABLE for async Python projects.

### Required Patterns

- **DO** use proper `async def` functions with `await`
- **DO** use `time.monotonic()` for timing
- **DO** use async-native libraries (aiohttp, asyncpg, etc.)

### Forbidden Patterns

- **DO NOT** use `asyncio.get_event_loop()` (deprecated Python 3.10+)
- **DO NOT** store event loop references (`self._loop = loop`)
- **DO NOT** use `asyncio.get_running_loop()` to store for later use

## Testing Requirements (NON-NEGOTIABLE)

Comprehensive testing before completion. This is NON-NEGOTIABLE.

### Requirements

- **E2E tests required** before marking any feature complete
- **Local testing first** before pushing to CI (save compute credits)
- **Test coverage target**: 90%+ for production code
- **All tests must pass** before commit

### Testing Workflow

1. Write tests for new functionality
2. Run tests locally until passing
3. Run lint and type checks
4. Only then commit and push

## [SECTION_2_NAME]
<!-- Example: Additional Constraints, Security Requirements, Performance Standards, etc. -->

[SECTION_2_CONTENT]
<!-- Example: Technology stack requirements, compliance standards, deployment policies, etc. -->

## [SECTION_3_NAME]
<!-- Example: Development Workflow, Review Process, Quality Gates, etc. -->

[SECTION_3_CONTENT]
<!-- Example: Code review requirements, testing gates, deployment approval process, etc. -->

## Governance
<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

[GOVERNANCE_RULES]
<!-- Example: All PRs/reviews must verify compliance; Complexity must be justified; Use [GUIDANCE_FILE] for runtime development guidance -->

**Version**: [CONSTITUTION_VERSION] | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE]
<!-- Example: Version: 2.1.1 | Ratified: 2025-06-13 | Last Amended: 2025-07-16 -->
