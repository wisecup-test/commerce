---
version: 2
name: cross-cutting-mutations-affecting-commerce-session-sta-e5d6
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Mutations Affecting Commerce Session State Cart"
metadata:
  actual:
    adr_id: e5d6da17-8886-499a-bf92-f1acb21bca77
    status: accepted
    candidate_group_id: 200a08a3-c441-4571-bdcc-aa8c14b01867
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/**
---

# Centralized Commerce Data Access and Domain Module via lib/shopify: Mutations Affecting Commerce Session State Cart

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for server components, route handlers, server actions, metadata generators, and components querying, mutating, or rendering e-commerce domain resources.

### Rules

- **R-COMM-001** SHOULD: Mutations affecting commerce session state or cart collections coordinate session persistence and cache revalidation exclusively through the centralized module interfaces.

### Verify

```bash
# Discover and run the project static analysis and type-checking scripts to verify that all commerce imports reference the centralized module.
# Discover and execute the automated test suite to ensure commerce queries, mutations, and revalidation workflows pass without errors.
# Discover and run the dependency audit script to confirm resolution artifacts match project manifest constraints.
```

**Accept when:**
- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must adhere to architectural boundaries, and direct external commerce endpoint calls bypassing the centralized module will be blocked.
</enforcement>