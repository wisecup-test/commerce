---
version: 2
name: cross-cutting-cache-invalidation-tag-revalidation-perf-1a65
description: "Asynchronous Framework Request Context and Cache Module Adoption: Cache Invalidation Tag Revalidation Performed Through"
metadata:
  actual:
    adr_id: 1a65e568-b065-4ac1-a428-de17cd9bbfc0
    status: accepted
    candidate_group_id: e608c980-ba4d-4597-8582-d6f9665f77c8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Asynchronous Framework Request Context and Cache Module Adoption: Cache Invalidation Tag Revalidation Performed Through

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CACHE-001** SHOULD: Cache invalidation and tag revalidation SHOULD be performed through the framework cache utilities immediately following state-altering mutations.

### Verify

```bash
# Discover and execute the repository type-check script to verify asynchronous return types on all header and cookie access invocations.
# Discover and execute the repository linter script to detect un-awaited framework primitives or unauthorized direct request manipulations.
# Discover and run the project test suite covering server actions and integration boundaries to validate state and revalidation handling.
```

**Accept when:**
- Server action and data access modules interact with HTTP request headers and cookies strictly through asynchronous helper invocations.
- Cache revalidation and tag invalidation are handled via framework-level cache utilities rather than manual cache resets.
- Static verification checks pass without unhandled Promise rejections or direct header/cookie manipulation without awaiting the framework primitives.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>