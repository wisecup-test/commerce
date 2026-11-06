---
version: 2
name: cross-cutting-consumers-not-reimplement-duplicate-foot-c76f
description: "Adoption of components/layout/footer Internal Module: Consumers Not Reimplement Duplicate Footer Navigation"
metadata:
  actual:
    adr_id: c76f324c-5213-4a3e-a364-a9fe5ee6547c
    status: accepted
    candidate_group_id: 4355269a-a052-42a8-9719-a90a3d82fc2c
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/layout/footer Internal Module: Consumers Not Reimplement Duplicate Footer Navigation

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FOOTER-001** MUST_NOT: Consumers MUST_NOT reimplement duplicate footer navigation elements or inline layout structures outside the canonical module.

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
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis, linting checks, and peer code reviews enforce compliance.
</enforcement>