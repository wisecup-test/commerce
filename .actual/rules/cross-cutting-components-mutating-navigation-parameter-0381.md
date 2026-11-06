---
version: 2
name: cross-cutting-components-mutating-navigation-parameter-0381
description: "next/navigation URL Search Parameter State Management: Components Mutating Navigation Parameters Perform Url"
metadata:
  actual:
    adr_id: 0381b419-7e85-4c35-b8f4-95dfb4db6cea
    status: accepted
    candidate_group_id: 3c2ba949-bbf9-45a7-9231-0f1e5cbe1593
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/cart/**
      - components/layout/**
      - components/product/**
      - lib/shopify/**
---

# next/navigation URL Search Parameter State Management: Components Mutating Navigation Parameters Perform Url

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for interactive client components, catalog interfaces, search interfaces, and variant selection views requiring state persistence across page reloads and deep linking.

### Rules

- **R-NAV-001** MUST: Components mutating navigation parameters MUST perform URL search parameter updates using params.set or equivalent parameter mutation APIs on URL parameter instances prior to triggering navigation updates.
- **R-NAV-002** MANDATORY: Construct new parameter sets by cloning existing URL search parameters and applying mutations through parameter setter methods before updating the location.
- **R-NAV-003** MANDATORY: Normalize string inputs retrieved via parameter accessors to lowercase or standardized representations prior to performing variant or option comparisons.
- **R-NAV-004** MANDATORY: Enforce domain validation and fallback handling on all parameter values extracted through search parameter readers.

### Verify

```bash
# Discover and execute repository type check and static analysis suites
# (e.g., npm run typecheck or npx tsc --noEmit)

# Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows
# (e.g., npm test)
```

**Accept when:**
- Static analysis and type validation suites complete without type errors across all navigation parameter access calls.
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration pipeline linting, type verification scripts, and peer code review.
</enforcement>