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

These rules are ALWAYS ACTIVE for:
- UI modules requiring browser-specific APIs, DOM event listeners, or client state hooks.
- Interactive controls, dynamic filtering elements, and modal overlays managing local user interactions.

### Rules

- **R-RM-001** SHOULD: When integrating third-party component libraries that depend on browser runtime features without native boundary declarations, developers SHOULD wrap them in dedicated client boundary modules before importing them into server components.

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
