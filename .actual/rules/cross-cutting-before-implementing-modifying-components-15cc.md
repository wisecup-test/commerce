---
version: 2
name: cross-cutting-before-implementing-modifying-components-15cc
description: "React Adoption for Modular User Interface Components: Before Implementing Modifying Components That Depend"
metadata:
  actual:
    adr_id: 15cc666c-0de7-4924-ae55-ca86e4539303
    status: accepted
    candidate_group_id: 1bded5af-d97d-41cd-9040-a47ad78b92f6
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/search/filter/**
---

# React Adoption for Modular User Interface Components: Before Implementing Modifying Components That Depend

Scope: applies to `components/layout/search/filter/**`.

These rules are ALWAYS ACTIVE for:
- User interface layout and search filter components requiring visual rendering and interaction handling.
- Interactive client controls that manage local presentation state or synchronize with navigation parameters.

### Rules

- **R-LM-001** MUST: Before implementing or modifying components that depend on React, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify that all invoked APIs exist in that version's official documentation.

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
