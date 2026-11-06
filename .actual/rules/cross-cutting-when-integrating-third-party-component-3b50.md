---
version: 2
name: cross-cutting-when-integrating-third-party-component-3b50
description: "React Client Component Boundary Directive in Next.js: When Integrating Third Party Component Libraries"
metadata:
  actual:
    adr_id: 3b509b4a-292e-438d-b9ff-2490dc19230f
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

# React Client Component Boundary Directive in Next.js: When Integrating Third Party Component Libraries

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** SHOULD: When integrating third-party component libraries that depend on browser runtime features without native boundary declarations, developers SHOULD wrap them in dedicated client boundary modules before importing them into server components.

### Verify

```bash
# Discover and execute the repository linter script
# Discover and execute the repository type-checking script
# Discover and execute the repository automated test suite
```

**Accept when:**
- Static analysis and type validation pass with zero errors across all client and server module boundaries.
- Automated tests successfully exercise client-side interactions and verify state persistence across route transitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>