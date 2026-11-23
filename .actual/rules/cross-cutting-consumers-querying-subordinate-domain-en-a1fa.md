---
version: 2
name: cross-cutting-consumers-querying-subordinate-domain-en-a1fa
description: "lib/shopify In-Memory Data Access: Consumers Querying Subordinate Domain Entities Lib"
metadata:
  actual:
    adr_id: a1fabe5f-ec5a-4bf5-807d-ae795e45998e
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

# lib/shopify In-Memory Data Access: Consumers Querying Subordinate Domain Entities Lib

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for:
- Client and server components accessing nested items within composite domain models provided by lib/shopify
- Operations locating line items, variants, options, or sort configurations by unique identifier or option predicate

### Rules

- **R-DAP-001** MUST: Consumers querying subordinate domain entities from lib/shopify collections MUST perform in-memory predicate matching against retrieved collection arrays rather than issuing secondary remote network queries.

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
