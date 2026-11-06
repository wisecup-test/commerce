---
version: 2
name: cross-cutting-client-components-not-import-alternative-0281
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Client Components Not Import Alternative Icon"
metadata:
  actual:
    adr_id: 02818a81-0b91-4676-9355-923227f01f18
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

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Client Components Not Import Alternative Icon

Scope: applies to `components/cart/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HERO-001** SHOULD_NOT: Client components SHOULD NOT import alternative icon style variants from external libraries unless an explicit visual exception is documented.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it to validate that all icon imports resolve against the locked dependency version.
# Execute the repository linting and static analysis tasks to ensure no unauthorized icon libraries or unapproved inline vector graphics have been introduced.
```

**Accept when:**
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis and code review checks during the pull request validation process.
</enforcement>