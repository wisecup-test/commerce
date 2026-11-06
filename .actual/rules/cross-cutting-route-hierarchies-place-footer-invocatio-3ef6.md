---
version: 2
name: cross-cutting-route-hierarchies-place-footer-invocatio-3ef6
description: "Adoption of components/layout/footer Internal Module: Route Hierarchies Place Footer Invocations Highest"
metadata:
  actual:
    adr_id: 3ef6807f-5f2d-4433-ba21-f6eb4a165074
    status: accepted
    candidate_group_id: 4355269a-a052-42a8-9719-a90a3d82fc2c
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/layout/footer Internal Module: Route Hierarchies Place Footer Invocations Highest

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all top-level application pages, nested route layouts, and route-level views requiring global footer presentation.

### Rules

- **R-FOOTER-001** SHOULD: Route hierarchies SHOULD place footer invocations in the highest applicable layout boundary to avoid redundant remounting across route transitions.
- **R-FOOTER-002** MANDATORY: Discover and adhere to the internal module aliasing conventions established in the repository configuration.
- **R-FOOTER-003** MANDATORY: Ensure all props exposed by the shared module maintain backward compatibility across route consumers.
- **R-FOOTER-004** MANDATORY: Execute lock-version grounding procedures (manifest lookup, build tool identification, lock/resolution inspection, documentation fetch, and API confirmation) prior to writing code using versioned libraries.

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