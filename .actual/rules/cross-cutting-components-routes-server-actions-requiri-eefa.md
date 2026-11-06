---
version: 2
name: cross-cutting-components-routes-server-actions-requiri-eefa
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Components Routes Server Actions Requiring Commerce"
metadata:
  actual:
    adr_id: eefa7f98-1ce0-4695-a83f-01f0d79b2f82
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

# Centralized Commerce Data Access and Domain Module via lib/shopify: Components Routes Server Actions Requiring Commerce

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for server components, route handlers, server actions, metadata generators, and components rendering catalog items, navigation links, cart state, or dynamic promotional displays requiring commerce data models.

### Rules

- **R-COMMERCE-001** MUST: Components, routes, and server actions requiring commerce data models MUST import domain schemas and types directly from the centralized module domain export.

### Verify

```bash
# Discover and run the project static analysis and type-checking scripts
npm run check
# Discover and execute the automated test suite
npm test
```

**Accept when:**
- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>