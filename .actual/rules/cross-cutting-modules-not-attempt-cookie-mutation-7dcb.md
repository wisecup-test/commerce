---
version: 2
name: cross-cutting-modules-not-attempt-cookie-mutation-7dcb
description: "Server-Side Request Context and Cookie Management via next/headers: Modules Not Attempt Cookie Mutation Outside"
metadata:
  actual:
    adr_id: 7dcbbca5-2de1-4da0-8822-807c390ecf53
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

# Server-Side Request Context and Cookie Management via next/headers: Modules Not Attempt Cookie Mutation Outside

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server action handlers, service boundary modules, and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-ADR-001** MUST_NOT: Modules MUST NOT attempt cookie mutation outside of designated server action mutations or framework-supported route execution lifecycles.

### Verify

```bash
# Discover and execute the project type-checking, linting, and test scripts from the repository configuration.
# Example discovery:
npm run --if-present typecheck
npm run --if-present lint
npm test
```

**Accept when:**
- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>