---
version: 2
name: cross-cutting-components-not-duplicate-url-search-b280
description: "next/navigation URL Search Parameter State Management: Components Not Duplicate Url Search Parameter"
metadata:
  actual:
    adr_id: b28091d5-6ff6-459a-a920-2f660503cf02
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

# next/navigation URL Search Parameter State Management: Components Not Duplicate Url Search Parameter

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST_NOT: Components MUST NOT duplicate URL search parameter values into detached client state stores that can fall out of synchronization with the browser navigation history.

### Verify

```bash
# Discover and execute repository type check and static analysis suites
# Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows
```

**Accept when:**
- Static analysis and type validation suites complete without type errors across all navigation parameter access calls
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>