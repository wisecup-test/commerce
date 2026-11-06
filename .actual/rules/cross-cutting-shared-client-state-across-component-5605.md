---
version: 2
name: cross-cutting-shared-client-state-across-component-5605
description: "React Core Library Adoption: Shared Client State Across Component Hierarchies"
metadata:
  actual:
    adr_id: 5605c55f-ff79-4c5e-9bed-37aed5959064
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

# React Core Library Adoption: Shared Client State Across Component Hierarchies

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** SHOULD: Shared client state across component hierarchies SHOULD use React context providers and reactive promise resolution primitives including the use hook rather than custom global event emitters.

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