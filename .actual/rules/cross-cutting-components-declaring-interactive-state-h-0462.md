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

These rules are ALWAYS ACTIVE for:
- User interface layout and search filter components requiring visual rendering and interaction handling.
- Interactive client controls that manage local presentation state or synchronize with navigation parameters.

### Rules

- **R-LM-001** MUST: Components declaring interactive state hooks MUST isolate client execution by declaring explicit client component boundaries.

### Verify

- Discover and execute the repository static analysis and component verification script declared in the project configuration.
- Discover and run the project test suite to verify component rendering and state transitions pass without errors.

**Accept when:**

- All user interface components compile without type or runtime dependency errors referencing the core UI library.
- Component state transitions and rendering logic adhere strictly to declarative hook lifecycle rules verified by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and linting passes integrated into continuous integration pipelines.
- Verified by: Peer code review verifying declarative hook usage and component boundary structure.
- Violation handling: Pull requests introducing imperative DOM modifications or untyped UI bindings are blocked from merging until refactored.
- Exception process: Exceptions must be submitted via architectural review with documented justification for non-declarative UI integration.
</enforcement>
