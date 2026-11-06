---
version: 2
name: cross-cutting-components-consuming-next-image-define-7deb
description: "Adoption of next/image for UI Image Presentation: Components Consuming Next Image Define Explicit"
metadata:
  actual:
    adr_id: 7debc22d-ff7d-41fc-af49-2e1ec4adbef1
    status: accepted
    candidate_group_id: e06a20a8-3618-46f3-9226-1b35388989a5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/grid/**
      - components/product/**
---

# Adoption of next/image for UI Image Presentation: Components Consuming Next Image Define Explicit

Scope: applies to `components/grid/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend user interface components rendering static or dynamic raster images, and product gallery and grid tile presentation components.

### Rules

- **R-IMG-001** SHOULD: Components consuming next/image SHOULD define explicit layout and aspect ratio constraints on enclosing containers.

### Verify

```bash
# Discover and execute project test commands to validate component rendering test suites
# Discover and run project static analysis scripts to verify imports and image component usage
```

**Accept when:**
- Visual display components render image assets exclusively through the next/image module.
- Project automated static analysis and test validation suites pass without unmanaged image tag warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>