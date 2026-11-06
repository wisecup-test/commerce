---
version: 2
name: cross-cutting-consumer-layouts-not-bypass-components-fc8e
description: "Adoption of components/grid/tile Shared Presentation Module: Consumer Layouts Not Bypass Components Grid"
metadata:
  actual:
    adr_id: fc8ef0b8-4bba-48d9-aa56-5433d1791c33
    status: accepted
    candidate_group_id: f5522651-25ba-4ef5-8b2d-69cfef172d1b
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/product/**
      - components/grid/**
      - components/layout/**
      - components/product/**
---

# Adoption of components/grid/tile Shared Presentation Module: Consumer Layouts Not Bypass Components Grid

Scope: applies to `app/product/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GRID-001** MUST_NOT: Consumer layouts MUST NOT bypass the components/grid/tile module boundary to directly manipulate internal tile styling primitives or recreate duplicate presentation logic.

### Verify

```bash
echo "Discover the project dependency manifest and lock artifact to verify resolved environment configurations."
echo "Execute the repository static analysis and linting scripts to verify compliance with shared component boundaries."
echo "Execute the repository test suite to confirm visual tile integration and regression test passes across all dependent surfaces."
```

**Accept when:**
- All catalog and showcase presentations import and render the shared grid tile module without bespoke styling overrides
- Repository verification and static analysis pipelines pass without architecture boundary violations

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated static analysis, peer code review, and integration pipeline checks.
</enforcement>