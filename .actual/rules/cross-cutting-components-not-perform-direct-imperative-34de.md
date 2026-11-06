---
version: 2
name: cross-cutting-components-not-perform-direct-imperative-34de
description: "React Adoption for Modular User Interface Components: Components Not Perform Direct Imperative Dom"
metadata:
  actual:
    adr_id: 34dea77d-2763-4352-97cb-00d5dcd21833
    status: accepted
    candidate_group_id: 1bded5af-d97d-41cd-9040-a47ad78b92f6
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/search/filter/**
---

# React Adoption for Modular User Interface Components: Components Not Perform Direct Imperative Dom

Scope: applies to `components/layout/search/filter/**`.

These rules are ALWAYS ACTIVE for all user interface layout and search filter components requiring visual rendering and interaction handling.

### Rules

- **R-ACT-001** MUST_NOT: Components MUST_NOT perform direct imperative DOM mutations outside of standard React lifecycle and rendering abstractions.

### Verify

```bash
# Discover and execute the repository static analysis and component verification script declared in the project configuration.
# Discover and run the project test suite to verify component rendering and state transitions pass without errors.
```

**Accept when:**
- All user interface components compile without type or runtime dependency errors referencing the core UI library.
- Component state transitions and rendering logic adhere strictly to declarative hook lifecycle rules verified by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting passes, and peer code review.
</enforcement>