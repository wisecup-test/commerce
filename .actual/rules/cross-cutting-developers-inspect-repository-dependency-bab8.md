---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-bab8
description: "React Client Component Boundary Directive in Next.js: Developers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: bab84e55-6a24-47ec-8a2b-c8d86cf8e910
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

# React Client Component Boundary Directive in Next.js: Developers Inspect Repository Dependency Lock Artifact

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for UI modules requiring browser-specific APIs, DOM event listeners, or client state hooks, and interactive controls, dynamic filtering elements, and modal overlays managing local user interactions.

### Rules

- **R-CLIENT-001** MUST: Developers MUST inspect the repository dependency lock artifact to resolve the exact version of the framework and rendering libraries before implementing or modifying client component boundaries.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>