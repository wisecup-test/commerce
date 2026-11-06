---
version: 2
name: cross-cutting-developers-discover-project-dependency-l-18fa
description: "lib/shopify In-Memory Data Access: Developers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 18fa1aa5-5ffd-4db0-849f-affd64519a03
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

# lib/shopify In-Memory Data Access: Developers Discover Project Dependency Lock Artifact

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for client and server components accessing nested items within composite domain models provided by lib/shopify, and operations locating line items, variants, options, or sort configurations by unique identifier or option predicate.

### Rules

- **R-SHOP-001** MUST: Developers MUST discover the project dependency lock artifact and verify all resolved dependency versions prior to implementing or modifying data access routines.

### Verify

```bash
# Discover the project test runner and script definitions from the repository configuration, then execute the suite verifying data access operations.
# Discover the static analysis and type checking verification scripts from the project manifest and execute them across all modules.
```

**Accept when:**
- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and type checking within continuous integration pipelines and peer code review enforce these requirements.
</enforcement>