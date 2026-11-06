---
version: 2
name: cross-cutting-component-state-derived-calculations-lif-013c
description: "React Core Library Adoption: Component State Derived Calculations Lifecycle Side"
metadata:
  actual:
    adr_id: 013c7b89-3a9c-45d0-8ca4-c915c1dfe203
    status: accepted
    candidate_group_id: dd4cf757-029b-4c55-a073-09368a803835
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/**
---

# React Core Library Adoption: Component State Derived Calculations Lifecycle Side

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Component state, derived calculations, and lifecycle side effects MUST be coordinated through React hooks including useState, useEffect, and useMemo.

### Verify

```bash
# Discover and execute repository type-checking script
# Discover and execute repository static analysis and linting scripts
# Discover and execute repository test suite
```

**Accept when:**
- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type verification, and peer code review must validate component boundary placement, hook dependency completeness, and proper client directive usage.
</enforcement>