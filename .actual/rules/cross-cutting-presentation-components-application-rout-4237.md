---
version: 2
name: cross-cutting-presentation-components-application-rout-4237
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Presentation Components Application Route Handlers Not"
metadata:
  actual:
    adr_id: 42372ac7-6a00-4af5-81eb-1f9941793c08
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

# Centralized Commerce Data Access and Domain Module via lib/shopify: Presentation Components Application Route Handlers Not

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for server components, route handlers, server actions, metadata generators, and presentation components querying or mutating e-commerce domain resources.

### Rules

- **R-SHOPIFY-001** MUST_NOT: Presentation components and application route handlers MUST NOT define ad-hoc external commerce network requests or vendor-specific query payloads outside the dedicated module boundary.

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
Claude Code MUST NOT skip or defer verification. All pull requests containing direct external commerce endpoint calls or bypassing the centralized module will be blocked.
</enforcement>