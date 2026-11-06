---
version: 2
name: cross-cutting-developers-not-duplicate-inline-vector-774d
description: "Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Developers Not Duplicate Inline Vector Paths"
metadata:
  actual:
    adr_id: 774d7ef0-1ebd-4541-b7f8-3e061b50091a
    status: accepted
    candidate_group_id: 3b7fb158-9916-4e65-972c-4185a31a0e1f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Developers Not Duplicate Inline Vector Paths

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOGO-001** MUST_NOT: Developers MUST NOT duplicate inline vector paths, raw markup, or standalone image definitions of the corporate logo outside the ./icons/logo module.

### Verify

```bash
# Discover and execute project component testing and visual regression/linting suites from repository configuration
```

**Accept when:**
- All user interface components and metadata generators render brand assets exclusively through the shared ./icons/logo module with zero inline SVG duplicates.
- Component test suites and visual regression checks pass without layout shifts or rendering failures across both interactive and metadata generation contexts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>