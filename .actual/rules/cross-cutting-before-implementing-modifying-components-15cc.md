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

These rules are ALWAYS ACTIVE for user interface layout and search filter components requiring visual rendering, interaction handling, and local presentation state management.

### Rules

- **R-REACT-001** MUST: Before implementing or modifying components that depend on React, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify that all invoked APIs exist in that version's official documentation.
- **R-REACT-002** MUST: Follow the LOCK-VERSION GROUNDING procedure: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official version-specific documentation, and confirm every called API exists in that version.
- **R-REACT-003** MUST: Declare state hooks at the top level of component bodies without conditional branching, and adhere strictly to hook call order constraints and lifecycle rules.
- **R-REACT-004** SHOULD: Structure interactive controls into separate container and presentation layers to maintain clean separation between state management and visual rendering.
- **R-REACT-005** SHOULD: Keep interactive component boundaries as low in the component hierarchy as possible to minimize client bundle footprint.

### Verify

```bash
# Discover and execute the repository static analysis and component verification script
# (Command derived dynamically from project repository configuration)

# Run the project test suite to verify component rendering and state transitions pass without errors
```

**Accept when:**
- All user interface components compile without type or runtime dependency errors referencing the core UI library.
- Component state transitions and rendering logic adhere strictly to declarative hook lifecycle rules verified by static analysis.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing imperative DOM modifications or untyped UI bindings are blocked until refactored.
</enforcement>