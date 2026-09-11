# Adoption of `lib/shopify` for E-commerce Integration: Interactions Shopify Platform Routed Through Lib

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: All interactions with the Shopify platform MUST be routed through the `lib/shopify` module.

### Verify

```bash
# Inspect the project's dependency manifest to identify the `lib/shopify` module.
# Search the codebase for imports of `lib/shopify` and its sub-modules.
# Review API routes and UI components to confirm all Shopify interactions use `lib/shopify`.
```

**Accept when:**
- All Shopify-related imports originate from `lib/shopify` or its sub-modules.
- No direct calls to Shopify APIs are found outside of `lib/shopify`.
- The `lib/shopify` module is well-structured and documented.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>