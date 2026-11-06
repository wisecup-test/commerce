---
version: 2
name: cross-cutting-components-declared-without-client-direc-1303
description: "React Client Component State and Effect Hooks Pattern: Components Declared Without Client Directive Boundary"
metadata:
  actual:
    adr_id: 1303ff1c-a0be-4b31-8dd3-3b1495f807cb
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Components Declared Without Client Directive Boundary

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST_NOT: Components declared without the client directive boundary MUST NOT invoke interactive state or lifecycle hooks.

### Verify

```bash
# Discover the test script in the project manifest and execute the test runner to verify interactive component behavior.
npm test
# Discover the linting script in the project manifest and execute the static analyzer to check client directive and hook usage.
npm run lint
# Discover the build verification script in the project manifest and execute the compiler to confirm type safety and client boundary exports.
npm run build
```

**Accept when:**
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>