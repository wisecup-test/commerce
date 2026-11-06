---
version: 2
name: cross-cutting-components-requiring-dynamic-interaction-6f6c
description: "React Client Component State and Effect Hooks Pattern: Components Requiring Dynamic Interaction State Dom"
metadata:
  actual:
    adr_id: 6f6c844d-cad4-4a33-98a5-ee4b4c957d3f
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Components Requiring Dynamic Interaction State Dom

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all interactive UI components, client context providers, and files consuming state, event handlers, or effect hooks.

### Rules

- **R-CLI-001** MUST: Components requiring dynamic interaction state, DOM event handling, or client-side lifecycle synchronization MUST be designated with the client directive boundary at the top of the component module.
- **R-CLI-002** MUST: Every component consuming useState, useEffect, or useMemo includes the client directive at the top of the file before imports or code.
- **R-CLI-003** SHOULD: Structure client boundary components at leaf levels of the component hierarchy to keep the client JavaScript bundle minimal.

### Verify

```bash
# Discover and execute the test script from the project manifest
npm test
# Discover and execute the linting script from the project manifest
npm run lint
# Discover and execute the build verification script from the project manifest
npm run build
```

**Accept when:**
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>