---
version: 2
name: cross-cutting-components-integrating-icons-logo-within-e454
description: "Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Components Integrating Icons Logo Within Dynamic"
metadata:
  actual:
    adr_id: e4547776-bab8-428e-82b2-a9946e7058b3
    status: accepted
    candidate_group_id: 3b7fb158-9916-4e65-972c-4185a31a0e1f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Components Integrating Icons Logo Within Dynamic

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for rendering corporate or brand logo assets in client-side or server-rendered user interface components and generating dynamic visual metadata artifacts, including social media preview images.

### Rules

- **R-LOGO-001** SHOULD: Components integrating ./icons/logo within dynamic image generation contexts ensure passed styling attributes remain compatible with server-side rendering constraints.
- **R-LOGO-002** MANDATORY: Discover the dependency manifest, build tool, and exact resolved version in the project repository before writing code that uses a versioned library.
- **R-LOGO-003** MANDATORY: Look up official documentation, changelog, or public API reference for the exact resolved version, and confirm every API, class, or function exists before using it.

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