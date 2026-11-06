---
version: 2
name: cross-cutting-inbound-request-header-verification-topi-4ee3
description: "Server-Side Request Context and Cookie Management via next/headers: Inbound Request Header Verification Topic Inspection"
metadata:
  actual:
    adr_id: 4ee3201a-c6aa-4a12-b100-1fdce5c92780
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

# Server-Side Request Context and Cookie Management via next/headers: Inbound Request Header Verification Topic Inspection

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server action handlers, service boundary modules, and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-NXT-001** MUST: Inbound request header verification and topic inspection MUST use the headers accessor function provided by next/headers.

### Verify

```bash
# Discover and execute the project type-checking and linting scripts from the repository configuration
# Example typical discovery/execution (derive actual commands from repository manifest):
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>