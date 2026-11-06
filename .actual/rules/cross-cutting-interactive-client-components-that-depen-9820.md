---
version: 2
name: cross-cutting-interactive-client-components-that-depen-9820
description: "next/navigation URL Search Parameter State Management: Interactive Client Components That Depend View"
metadata:
  actual:
    adr_id: 98206118-2784-46d2-9d84-93c55d8c500b
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

# next/navigation URL Search Parameter State Management: Interactive Client Components That Depend View

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for interactive client components managing filter, sort, search, variant, or view selection parameters, and catalog and search interfaces that require deep linking and bookmark preservation.

### Rules

- **R-NAV-001** MUST: Interactive client components that depend on view filtering, sorting, variant selection, or pagination state MUST source and synchronize that state through next/navigation URL search parameter access APIs rather than unpersisted client memory stores.

### Verify

```bash
# Discover and execute repository type check and static analysis suites to confirm all query parameter reads and writes conform to framework navigation typings
# Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows
```

**Accept when:**
- Static analysis and type validation suites complete without type errors across all navigation parameter access calls
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>