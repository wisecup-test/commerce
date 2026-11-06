---
version: 2
name: cross-cutting-before-consuming-modifying-versioned-dep-472e
description: "Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Before Consuming Modifying Versioned Dependencies Shared"
metadata:
  actual:
    adr_id: 472e5638-02cf-4e7d-ad0c-589da370c42c
    status: accepted
    candidate_group_id: 3b7fb158-9916-4e65-972c-4185a31a0e1f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Before Consuming Modifying Versioned Dependencies Shared

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOGO-001** MUST: Before consuming or modifying versioned dependencies or shared internal modules, developers MUST inspect the repository lock artifact to verify resolved interface definitions and compatibility.
- **R-LOGO-002** MUST: Render corporate or brand logo assets in client-side or server-rendered user interface components and dynamic visual metadata artifacts exclusively through the shared `./icons/logo` module.
- **R-LOGO-003** MUST: Keep `./icons/logo` pure and presentational without client-only state, hooks, or browser-specific globals to ensure compatibility with server-side image generation environments.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute component testing across all visual modules importing ./icons/logo.
# Execute the repository visual regression and linting suites discovered in the project automation pipeline to validate brand rendering integrity.
```

**Accept when:**
- All user interface components and metadata generators render brand assets exclusively through the shared ./icons/logo module with zero inline SVG duplicates.
- Component test suites and visual regression checks pass without layout shifts or rendering failures across both interactive and metadata generation contexts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>