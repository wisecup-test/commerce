---
version: 2
name: cross-cutting-components-declaring-interactive-state-h-0462
description: "React Adoption for Modular User Interface Components: Components Declaring Interactive State Hooks Isolate"
metadata:
  actual:
    adr_id: 04627e50-ff05-4ffa-8e04-c871433abfc1
    status: accepted
    candidate_group_id: 1bded5af-d97d-41cd-9040-a47ad78b92f6
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/search/filter/**
---

# React Adoption for Modular User Interface Components: Components Declaring Interactive State Hooks Isolate

Scope: applies to `components/layout/search/filter/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Components declaring interactive state hooks MUST isolate client execution by declaring explicit client component boundaries.

### Verify

```bash
# Discover and execute repository static analysis and component verification script declared in project configuration
# Discover and run project test suite to verify component rendering and state transitions pass without errors
```

**Accept when:**
- All user interface components compile without type or runtime dependency errors referencing the core UI library.
- Component state transitions and rendering logic adhere strictly to declarative hook lifecycle rules verified by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>