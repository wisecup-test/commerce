---
version: 2
name: cross-cutting-modules-not-bypass-framework-asynchronou-eae9
description: "Asynchronous Framework Request Context and Cache Module Adoption: Modules Not Bypass Framework Asynchronous Cookie"
metadata:
  actual:
    adr_id: eae9dc32-4808-4646-b729-fc0b7a2d09f0
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

# Asynchronous Framework Request Context and Cache Module Adoption: Modules Not Bypass Framework Asynchronous Cookie

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all server actions, data fetching routines, and external API client adapters running in server execution contexts requiring request header or cookie access, as well as modules handling server-side cache revalidation and webhook payload processing.

### Rules

- **R-CTX-001** MUST_NOT: Modules MUST NOT bypass the framework asynchronous cookie layer by setting or reading raw response headers directly in server actions.

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