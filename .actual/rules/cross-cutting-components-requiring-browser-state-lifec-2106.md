---
version: 2
name: cross-cutting-components-requiring-browser-state-lifec-2106
description: "React Client Component Boundary Directive in Next.js: Components Requiring Browser State Lifecycle Hooks"
metadata:
  actual:
    adr_id: 2106f018-f0f6-4cb3-ba19-c634b39f13ab
    status: accepted
    candidate_group_id: 00168ec8-5612-44f2-aa12-79742c72bc98
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/**
---

# React Client Component Boundary Directive in Next.js: Components Requiring Browser State Lifecycle Hooks

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CCB-001** MUST: Components requiring browser state, lifecycle hooks, event listeners, or client navigation APIs MUST explicitly declare the client component boundary directive at the top of the source module before any import statements.

### Verify

```bash
# Discover and execute the repository linter script to ensure client boundary directives comply with project static analysis and syntax standards.
# Discover and execute the repository type-checking script to validate module boundaries and imported hook types across client and server files.
# Discover and execute the repository automated test suite to verify interactive component state transitions and rendering behavior.
```

**Accept when:**
- Static analysis and type validation pass with zero errors across all client and server module boundaries.
- Automated tests successfully exercise client-side interactions and verify state persistence across route transitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>