---
version: 2
name: cross-cutting-server-actions-data-integration-modules-01d0
description: "Server-Side Request Context and Cookie Management via next/headers: Server Actions Data Integration Modules Requiring"
metadata:
  actual:
    adr_id: 01d0a680-9352-4696-b592-a595eedaffc8
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

# Server-Side Request Context and Cookie Management via next/headers: Server Actions Data Integration Modules Requiring

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server action handlers executing server-side state transitions, and service boundary modules and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-CTX-001** MUST: Server actions and data integration modules requiring HTTP request header inspection or cookie management MUST consume the next/headers module for request context access.

### Verify

```bash
# Discover and execute the project type-checking and linting scripts from the repository configuration to verify all next/headers imports and accessor signatures satisfy contract requirements.
# Discover and run the automated test suite configured in the repository manifest to verify server actions and integration handlers appropriately handle header and cookie state.
```

**Accept when:**
- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>