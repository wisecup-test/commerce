---
version: 2
name: cross-cutting-derived-state-computations-across-unwrap-3050
description: "React Promise Unwrapping via use API at Context Boundaries: Derived State Computations Across Unwrapped Collection"
metadata:
  actual:
    adr_id: 3050359f-2c41-4abb-8852-6d43c76b2d2f
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Derived State Computations Across Unwrapped Collection

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for client components and context consumer hooks consuming asynchronous context promises, and context providers encapsulating asynchronous resource promises for subtree consumption.

### Rules

- **R-REACT-001** SHOULD: Derived state computations across unwrapped collection items SHOULD utilize useMemo to prevent redundant transformations across component re-renders.

### Verify

```bash
# Discover the project test runner from the repository manifest and execute component test suites verifying context promise resolution.
# Discover the static analysis verification command from the repository configuration and validate hook usage rules across context consumer modules.
```

**Accept when:**
- All context consumer components unwrapping promises compile and pass static verification without hook rule violations.
- Component tests verify successful data unwrapping and proper fallback handling during pending promise states.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>