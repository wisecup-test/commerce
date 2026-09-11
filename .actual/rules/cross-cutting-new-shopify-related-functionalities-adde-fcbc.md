# Adoption of `lib/shopify` for E-commerce Integration: New Shopify Related Functionalities Added Lib

These rules are ALWAYS ACTIVE for all code interacting with Shopify APIs or data, including UI components and API routes.

### Rules

- R-SHOPIFY-001 MAY: New Shopify-related functionalities MAY be added to `lib/shopify` following existing patterns within the module.

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