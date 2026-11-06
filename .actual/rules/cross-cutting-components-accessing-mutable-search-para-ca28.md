---
version: 2
name: cross-cutting-components-accessing-mutable-search-para-ca28
description: "React Client Component Boundary Directive in Next.js: Components Accessing Mutable Search Parameters Navigation"
metadata:
  actual:
    adr_id: ca284259-c96e-4d56-a80c-fea2e9126f72
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

# React Client Component Boundary Directive in Next.js: Components Accessing Mutable Search Parameters Navigation

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CCB-001** MUST: Components accessing mutable search parameters or navigation router events on the client MUST use client navigation hooks within modules marked with the client component boundary directive.

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
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis pipelines validate boundary directive placement and cross-boundary import patterns.
</enforcement>