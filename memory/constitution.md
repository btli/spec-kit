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
