---
version: 2
name: cross-cutting-interactive-presentation-components-that-444a
description: "React Core Library Adoption: Interactive Presentation Components That Manage Internal"
metadata:
  actual:
    adr_id: 444a8301-fdf3-4863-9ac4-d1d278c281fd
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

# React Core Library Adoption: Interactive Presentation Components That Manage Internal

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Interactive presentation components that manage internal state or execute lifecycle effects MUST declare the client runtime directive at the file boundary before invoking React hooks.

### Verify

```bash
# Discover and execute the repository type-checking script to validate React component signatures, hook parameter types, and context definitions across all presentation modules.
# Discover and execute the repository static analysis and linting scripts to verify compliance with component rendering directives and hook dependency guidelines.
# Discover and execute the repository test suite to confirm that reactive components and context providers render and transition state as expected.
```

**Accept when:**
- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>