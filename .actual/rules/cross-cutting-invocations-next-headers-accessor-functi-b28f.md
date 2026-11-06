---
version: 2
name: cross-cutting-invocations-next-headers-accessor-functi-b28f
description: "Server-Side Request Context and Cookie Management via next/headers: Invocations Next Headers Accessor Functions Encapsulated"
metadata:
  actual:
    adr_id: b28f5082-ff45-4ca5-8831-9bd497483596
    status: accepted
    candidate_group_id: 0e0e1f68-e5a9-4e8a-aec5-00713a779cbb
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Server-Side Request Context and Cookie Management via next/headers: Invocations Next Headers Accessor Functions Encapsulated

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server action handlers, service boundary modules, and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-HDR-001** SHOULD: Invocations of next/headers accessor functions SHOULD be encapsulated within dedicated action handlers or service boundary facades rather than mixed into rendering logic.

### Verify

```bash
# Discover and execute the project type-checking and linting scripts from the repository configuration
npm run lint || pnpm lint || yarn lint
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and run the automated test suite
npm test || pnpm test || yarn test
```

**Accept when:**
- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>