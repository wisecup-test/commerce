---
version: 2
name: cross-cutting-shared-components-opengraph-image-module-367c
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Shared Components Opengraph Image Module Maintain"
metadata:
  actual:
    adr_id: 367c796f-4cea-423f-b42c-7b48a4734dd8
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Shared Components Opengraph Image Module Maintain

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OG-001** MUST: The shared `components/opengraph-image` module MUST maintain a stable presentation interface accepting parameterized title and context inputs across consuming route handlers.

### Verify

```bash
# Discover repository build and static analysis commands from package manifests and run lint checks
# Discover and execute project test runner to verify social image generation handlers resolve and execute
```

**Accept when:**
- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review checks during continuous integration enforce compliance.
</enforcement>