---
version: 2
name: cross-cutting-client-components-reading-url-search-be8c
description: "next/navigation URL Search Parameter State Management: Client Components Reading Url Search Parameters"
metadata:
  actual:
    adr_id: be8ca393-7232-4e00-9c37-01d3c969346b
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

# next/navigation URL Search Parameter State Management: Client Components Reading Url Search Parameters

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all interactive client components managing filter, sort, search, variant, or view selection parameters, and catalog and search interfaces requiring deep linking and bookmark preservation.

### Rules

- **R-NAV-001** MUST: Client components reading URL search parameters MUST access query keys through searchParams.get to evaluate active selection parameters.

### Verify

```bash
# Discover and execute repository type check and static analysis suites
# Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows
```

**Accept when:**
- Static analysis and type validation suites complete without type errors across all navigation parameter access calls
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline executing repository linting and type verification scripts, and peer code review auditing all new interactive view components.
</enforcement>