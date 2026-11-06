---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-a501
description: "Adoption of next/image for UI Image Presentation: Engineers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: a5011864-01e3-42bc-8511-10f5a525e4eb
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

# Adoption of next/image for UI Image Presentation: Engineers Discover Repository Dependency Manifest Inspect

Scope: applies to `components/grid/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend user interface components rendering static or dynamic raster images, product gallery, and grid tile presentation components.

### Rules

- **R-IMG-001** MUST: Engineers MUST discover the repository dependency manifest and inspect the resolution artifact to verify the exact resolved version before implementing next/image components.

### Verify

```bash
# Discover and execute project test commands to validate component rendering test suites
npm test

# Discover and run project static analysis scripts to verify imports and image component usage
npm run lint
```

**Accept when:**
- Visual display components render image assets exclusively through the next/image module.
- Project automated static analysis and test validation suites pass without unmanaged image tag warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and peer code review.
</enforcement>