---
version: 2
name: cross-cutting-developers-push-client-component-boundar-6def
description: "React Client Component Boundary Directive in Next.js: Developers Push Client Component Boundaries Leaf"
metadata:
  actual:
    adr_id: 6def2013-700c-4d9c-b470-81520fe15112
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

# React Client Component Boundary Directive in Next.js: Developers Push Client Component Boundaries Leaf

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all UI modules, interactive controls, dynamic filtering elements, modal overlays, data-fetching page layouts, root route segments, and static presentational components within the repository.

### Rules

- **R-CLI-001** SHOULD: Push client component boundaries to leaf nodes of the component hierarchy to maximize server-side rendering and minimize client bundle size.

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