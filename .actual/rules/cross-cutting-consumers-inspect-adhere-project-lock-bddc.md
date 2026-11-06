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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FOOTER-001** MUST: Consumers MUST inspect and adhere to the project lock and resolution artifact to resolve all internal module paths and dependencies accurately before implementation.
- **R-FOOTER-002** MUST: Discover and adhere to the internal module aliasing conventions established in the repository configuration.
- **R-FOOTER-003** MUST: Ensure all props exposed by the shared module maintain backward compatibility across route consumers.

### Verify

```bash
# Discover and execute repository linting and static analysis scripts to verify adherence to module import constraints.
# Discover and execute repository automated test suites to validate layout rendering and integration integrity.
# Discover and execute repository type-checking scripts to verify type compatibility across module consumer boundaries.
```

**Accept when:**
- Static analysis validates that all footer references resolve directly to the centralized layout module.
- Automated layout and integration tests execute successfully without regressions across all affected views.
- Type checking completes with zero diagnostic errors across consumer and module boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>