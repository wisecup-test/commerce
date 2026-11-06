---
version: 2
name: cross-cutting-components-not-embed-raw-inline-3f8e
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Components Not Embed Raw Inline Svg"
metadata:
  actual:
    adr_id: 3f8ea0f5-c7ac-4cd8-92a3-e497fcd81d38
    status: accepted
    candidate_group_id: 79132c3d-c9da-4982-ba2a-7e35341d6aea
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/layout/**
      - components/product/**
---

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Components Not Embed Raw Inline Svg

Scope: applies to `components/cart/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all user interface components displaying decorative or action-associated iconography in navigation, search, cart, and product views.

### Rules

- **R-HERO-001** MUST_NOT: Components MUST NOT embed raw inline SVG element structures for icons when an equivalent symbol exists within @heroicons/react/24/outline.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it
# to validate that all icon imports resolve against the locked dependency version.
# Also execute repository linting and static analysis tasks.
npm test || yarn test || pnpm test || bun test
```

**Accept when:**
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, code review checks, and dependency audit tools will block pull requests containing unauthorized icon imports or raw inline SVG icons without an approved exception.
</enforcement>