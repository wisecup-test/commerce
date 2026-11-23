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

These rules are ALWAYS ACTIVE for:
- Interactive client components managing filter, sort, search, variant, or view selection parameters
- Catalog and search interfaces that require deep linking and bookmark preservation

### Rules

- **R-PD-001** MUST: Client components reading URL search parameters MUST access query keys through searchParams.get to evaluate active selection parameters.

### Verify

- Discover and execute repository type check and static analysis suites to confirm all query parameter reads and writes conform to framework navigation typings
- Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows

**Accept when:**

- Static analysis and type validation suites complete without type errors across all navigation parameter access calls
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated continuous integration pipeline executing repository linting and type verification scripts
- Verified by: Peer code review auditing all new interactive view components for compliance with URL search parameter state access rules
- Violation handling: Code reviews block merge requests that introduce detached in-memory state for shareable catalog or search filters
- Violation handling: Continuous integration checks fail on unhandled navigation parameter types
- Exception process: Architectural review submission documenting why a specific state domain cannot be serialized into URL parameters
</enforcement>
