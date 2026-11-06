---
version: 2
name: cross-cutting-parameters-read-via-searchparams-get-16c8
description: "next/navigation URL Search Parameter State Management: Parameters Read Via Searchparams Get Validated"
metadata:
  actual:
    adr_id: 16c8e883-72c3-4a0d-ac6f-732533bfdf0d
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

# next/navigation URL Search Parameter State Management: Parameters Read Via Searchparams Get Validated

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for interactive client components managing filter, sort, search, variant, or view selection parameters, and catalog/search interfaces requiring deep linking and bookmark preservation.

### Rules

- **R-NAV-001** SHOULD: Parameters read via searchParams.get SHOULD be validated against expected schema domains before driving product variant resolution or catalog query execution.

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