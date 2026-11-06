---
version: 2
name: cross-cutting-client-components-bind-server-action-de31
description: "React Client Component Boundary Directive in Next.js: Client Components Bind Server Action Handlers"
metadata:
  actual:
    adr_id: de31dce2-d099-48e3-8181-2024603a80e3
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

# React Client Component Boundary Directive in Next.js: Client Components Bind Server Action Handlers

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for UI modules requiring browser-specific APIs, DOM event listeners, or client state hooks, and interactive controls, dynamic filtering elements, and modal overlays managing local user interactions.

### Rules

- **R-CCC-001** MAY: Client components MAY bind server action handlers directly to interactive form controls and event callbacks to trigger server-side state mutations.

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