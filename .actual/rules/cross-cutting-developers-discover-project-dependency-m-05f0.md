---
version: 2
name: cross-cutting-developers-discover-project-dependency-m-05f0
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Developers Discover Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 05f04f18-7f15-4946-9c71-2dbada14da64
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

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Developers Discover Project Dependency Manifest Lock

Scope: applies to `components/cart/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HERO-001** MUST: Developers MUST discover the project dependency manifest and lock artifact to resolve the exact locked version of the iconography library before implementing new icon references.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest
# Example pattern (adapt to repository specifics):
npm test
npm run lint
```

**Accept when:**
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>