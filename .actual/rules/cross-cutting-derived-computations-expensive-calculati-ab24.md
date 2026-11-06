---
version: 2
name: cross-cutting-derived-computations-expensive-calculati-ab24
description: "React Client Component State and Effect Hooks Pattern: Derived Computations Expensive Calculations Memoized Usememo"
metadata:
  actual:
    adr_id: ab24e2f0-9458-4d47-b308-c0882d72a187
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Derived Computations Expensive Calculations Memoized Usememo

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for interactive UI components handling user input, modal visibility, menu state, navigation filter interactions, and client context providers.

### Rules

- **R-RCS-001** SHOULD: Derived computations and expensive calculations SHOULD be memoized using useMemo to avoid redundant evaluations across re-renders.
- **R-RCS-002** MANDATORY: Every component consuming useState, useEffect, or useMemo includes the client directive at the top of the file before imports or code.
- **R-RCS-003** MANDATORY: Structure client boundary components at leaf levels of the component hierarchy to keep the client JavaScript bundle minimal.

### Verify

```bash
# Discover and execute the test runner to verify interactive component behavior
# Discover and execute the static analyzer to check client directive and hook usage
# Discover and execute the compiler to confirm type safety and client boundary exports
```

**Accept when:**
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>