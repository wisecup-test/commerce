---
version: 2
name: cross-cutting-memory-collection-searches-validate-enti-f885
description: "lib/shopify In-Memory Data Access: Memory Collection Searches Validate Entity Presence"
metadata:
  actual:
    adr_id: f8852df2-c483-4f07-986e-4d2f3632b096
    status: accepted
    candidate_group_id: 0c82ddb0-1946-4982-a199-cb6a1a72d36a
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/cart/**
      - components/product/**
---

# lib/shopify In-Memory Data Access: Memory Collection Searches Validate Entity Presence

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for:
- Client and server components accessing nested items within composite domain models provided by lib/shopify
- Operations locating line items, variants, options, or sort configurations by unique identifier or option predicate

### Rules

- **R-DAP-001** MUST: In-memory collection searches MUST validate entity presence and handle null or undefined results when matching identifiers or composite option criteria.

### Verify

- Discover the project test runner and script definitions from the repository configuration, then execute the suite verifying data access operations.
- Discover the static analysis and type checking verification scripts from the project manifest and execute them across all modules.

**Accept when:**

- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and type checking within continuous integration pipelines.
- Verified by: Peer code review verifying correct predicate matching and null-handling on collection searches.
- Violation handling: Static analysis or type errors block merge requests until resolved.
- Violation handling: Unmitigated runtime access or redundant network query implementations must be refactored during review.
- Exception process: Exceptions requiring normalized client-side indices or dedicated remote sub-resource queries require architectural review documentation and team lead approval.
</enforcement>
