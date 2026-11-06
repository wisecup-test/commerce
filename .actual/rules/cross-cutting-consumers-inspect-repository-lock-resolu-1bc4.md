---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-resolu-1bc4
description: "Asynchronous Framework Request Context and Cache Module Adoption: Consumers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 1bc4b306-ab2c-4baf-8e9e-fdf9e36f3c7f
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

# Asynchronous Framework Request Context and Cache Module Adoption: Consumers Inspect Repository Lock Resolution Artifact

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AC-001** MUST: Consumers MUST inspect the repository lock or resolution artifact to determine the exact resolved framework version before implementing asynchronous header or cookie calls.

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