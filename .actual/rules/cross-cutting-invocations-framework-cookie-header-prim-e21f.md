---
version: 2
name: cross-cutting-invocations-framework-cookie-header-prim-e21f
description: "Asynchronous Framework Request Context and Cache Module Adoption: Invocations Framework Cookie Header Primitives Awaited"
metadata:
  actual:
    adr_id: e21f16d0-0284-46eb-b7d3-4a4ef54b7b15
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

# Asynchronous Framework Request Context and Cache Module Adoption: Invocations Framework Cookie Header Primitives Awaited

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- All server actions, data fetching routines, and external API client adapters running in server execution contexts requiring request header or cookie access.
- Modules handling server-side cache revalidation and webhook payload processing.

### Rules

- **R-LM-001** MUST: All invocations of the framework cookie and header primitives MUST be awaited prior to accessing or mutating values.

### Verify

- Discover and execute the repository type-check script to verify asynchronous return types on all header and cookie access invocations.
- Discover and execute the repository linter script to detect un-awaited framework primitives or unauthorized direct request manipulations.
- Discover and run the project test suite covering server actions and integration boundaries to validate state and revalidation handling.

**Accept when:**

- Server action and data access modules interact with HTTP request headers and cookies strictly through asynchronous helper invocations.
- Cache revalidation and tag invalidation are handled via framework-level cache utilities rather than manual cache resets.
- Static verification checks pass without unhandled Promise rejections or direct header/cookie manipulation without awaiting the framework primitives.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated linting rules checking for proper asynchronous usage of framework cookie and header access utilities.
- Verified by: Pull request peer review validating server action signatures and cache boundary isolation.
- Violation handling: Pull requests containing un-awaited framework header or cookie calls will be blocked by automated checks.
- Violation handling: Violations identified during code review will require refactoring to use framework server access primitives before merging.
- Exception process: Exceptions require review by the architecture team with documented justification demonstrating why asynchronous framework utilities cannot satisfy the use case.
- Exception process: Temporary exemptions must include a scheduled migration ticket to align with standard server primitives.
</enforcement>
