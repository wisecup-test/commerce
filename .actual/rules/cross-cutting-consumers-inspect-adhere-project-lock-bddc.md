---
version: 2
name: cross-cutting-consumers-inspect-adhere-project-lock-bddc
description: "Adoption of components/layout/footer Internal Module: Consumers Inspect Adhere Project Lock Resolution"
metadata:
  actual:
    adr_id: bddc6cbc-d4c3-4345-bb26-ed7d7b09c8a7
    status: accepted
    candidate_group_id: 4355269a-a052-42a8-9719-a90a3d82fc2c
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/layout/footer Internal Module: Consumers Inspect Adhere Project Lock Resolution

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for:
- All top-level application pages and nested route layouts that require global footer presentation.
- All route-level views sharing common application chrome and navigation structures.

### Rules

- **R-LM-001** MUST: Consumers MUST inspect and adhere to the project lock and resolution artifact to resolve all internal module paths and dependencies accurately before implementation.

### Verify

- Discover and execute repository linting and static analysis scripts to verify adherence to module import constraints.
- Discover and execute repository automated test suites to validate layout rendering and integration integrity.
- Discover and execute repository type-checking scripts to verify type compatibility across module consumer boundaries.

**Accept when:**

- Static analysis validates that all footer references resolve directly to the centralized layout module.
- Automated layout and integration tests execute successfully without regressions across all affected views.
- Type checking completes with zero diagnostic errors across consumer and module boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Continuous integration automated static analysis and linting checks.
- Verified by: Peer code review required for all pull requests modifying route templates or layout structures.
- Violation handling: Pull requests introducing duplicated footer markup or unapproved layout components are rejected during review.
- Violation handling: Discovered violations must be refactored to consume the centralized module prior to integration.
- Exception process: Exceptions require written architectural review and approval from lead engineering before implementation.
- Exception process: Documented exceptions must define a bounded duration and be tracked in the project backlog for eventual consolidation.
</enforcement>
