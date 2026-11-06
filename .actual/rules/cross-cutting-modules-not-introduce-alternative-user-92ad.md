---
version: 2
name: cross-cutting-modules-not-introduce-alternative-user-92ad
description: "React Core Library Adoption: Modules Not Introduce Alternative User Interface"
metadata:
  actual:
    adr_id: 92ada000-e925-4494-bbdb-a05013752806
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

# React Core Library Adoption: Modules Not Introduce Alternative User Interface

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for user interface components, layout wrappers, and interactive client context modules requiring declarative view rendering and local state management.

### Rules

- **R-REACT-001** MUST_NOT: Modules MUST NOT introduce alternative user interface rendering frameworks or imperative view manipulation libraries into the component hierarchy.

### Verify

```bash
# Discover and execute the repository type-checking script
# Discover and execute the repository static analysis and linting scripts
# Discover and execute the repository test suite
```

**Accept when:**
- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>