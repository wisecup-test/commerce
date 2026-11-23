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

These rules are ALWAYS ACTIVE for:
- UI modules requiring browser-specific APIs, DOM event listeners, or client state hooks.
- Interactive controls, dynamic filtering elements, and modal overlays managing local user interactions.

### Rules

- **R-RM-001** MUST: Components accessing mutable search parameters or navigation router events on the client MUST use client navigation hooks within modules marked with the client component boundary directive.

### Verify

- Discover and execute the repository linter script to ensure client boundary directives comply with project static analysis and syntax standards.
- Discover and execute the repository type-checking script to validate module boundaries and imported hook types across client and server files.
- Discover and execute the repository automated test suite to verify interactive component state transitions and rendering behavior.

**Accept when:**

- Static analysis and type validation pass with zero errors across all client and server module boundaries.
- Automated tests successfully exercise client-side interactions and verify state persistence across route transitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Continuous integration static analysis pipelines validating boundary directive placement and cross-boundary import patterns.
- Verified by: Peer code reviews inspecting component hierarchy depth to ensure client boundaries remain restricted to interactive leaves.
- Violation handling: Pull requests with misplaced client boundary directives or non-serializable property bindings are blocked until refactored.
- Exception process: Exceptions require documented justification and approval from the frontend architecture lead detailing why elevated boundary placement cannot be isolated to leaf nodes.
</enforcement>
