---
version: 2
name: cross-cutting-components-that-solely-perform-static-322c
description: "React Client Component Boundary Directive in Next.js: Components That Solely Perform Static Presentation"
metadata:
  actual:
    adr_id: 322c540e-c264-44dd-885b-25e0fad97b37
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

# React Client Component Boundary Directive in Next.js: Components That Solely Perform Static Presentation

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CCB-001** MUST_NOT: Components that solely perform static presentation, server-side data retrieval, or pass-through layout composition MUST NOT declare the client component boundary directive.

### Verify

```bash
# Discover and execute repository linter, type-checking, and test scripts per project configuration
```

**Accept when:**
- Static analysis and type validation pass with zero errors across all client and server module boundaries.
- Automated tests successfully exercise client-side interactions and verify state persistence across route transitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>