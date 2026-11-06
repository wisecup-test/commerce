---
version: 2
name: cross-cutting-commerce-domain-operations-data-retrieva-f488
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Commerce Domain Operations Data Retrieval State"
metadata:
  actual:
    adr_id: f48897de-8887-4eff-888a-bc57083db9ef
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

# Centralized Commerce Data Access and Domain Module via lib/shopify: Commerce Domain Operations Data Retrieval State

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COM-001** MUST: All commerce domain operations, data retrieval, and state mutations MUST route through the centralized internal commerce module rather than communicating directly with external commerce endpoints.

### Verify

```bash
# Discover and run the project static analysis and type-checking scripts
npm run typecheck
# Discover and execute the automated test suite
npm test
# Discover and run the dependency audit script
npm run audit
```

**Accept when:**
- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All commerce operations must be routed through the centralized module and verified via automated checks.
</enforcement>