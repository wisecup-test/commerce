---
version: 2
name: cross-cutting-dynamic-metadata-image-generation-routes-09f7
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Dynamic Metadata Image Generation Routes Consume"
metadata:
  actual:
    adr_id: 09f7bd14-7bf3-48be-875d-7e7d4ff9586a
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

# Centralized Commerce Data Access and Domain Module via lib/shopify: Dynamic Metadata Image Generation Routes Consume

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for server components, route handlers, server actions, metadata generators querying or mutating e-commerce domain resources, and components rendering catalog items, navigation links, cart state, or dynamic promotional displays.

### Rules

- **R-SHOPIFY-001** MAY: Dynamic metadata and image generation routes MAY consume specialized read-only query helpers exported by the module to construct contextual representations.

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
Claude Code MUST NOT skip or defer verification. All pull requests containing direct external commerce endpoint calls or bypassing the centralized module will be blocked.
</enforcement>