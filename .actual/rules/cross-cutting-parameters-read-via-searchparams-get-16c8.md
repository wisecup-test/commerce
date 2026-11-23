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

These rules are ALWAYS ACTIVE for:
- Interactive client components managing filter, sort, search, variant, or view selection parameters
- Catalog and search interfaces that require deep linking and bookmark preservation

### Rules

- **R-PD-001** SHOULD: Parameters read via searchParams.get SHOULD be validated against expected schema domains before driving product variant resolution or catalog query execution.

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
