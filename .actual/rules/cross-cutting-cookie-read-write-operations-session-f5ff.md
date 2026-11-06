---
version: 2
name: cross-cutting-cookie-read-write-operations-session-f5ff
description: "Server-Side Request Context and Cookie Management via next/headers: Cookie Read Write Operations Session Tracking"
metadata:
  actual:
    adr_id: f5ff3a52-d284-44a2-958e-d37f12cf2f17
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

# Server-Side Request Context and Cookie Management via next/headers: Cookie Read Write Operations Session Tracking

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server action handlers, service boundary modules, and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-COOKIE-001** MUST: Cookie read and write operations for session tracking MUST use the cookies accessor function from next/headers and await its return value in accordance with resolved framework specifications.

### Verify

```bash
# Discover and execute project type-checking and linting scripts from the repository configuration
# Discover and run the automated test suite configured in the repository manifest
```

**Accept when:**
- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>