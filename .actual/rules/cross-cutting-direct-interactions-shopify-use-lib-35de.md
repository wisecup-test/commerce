# Adoption and Usage Pattern of `lib/shopify` Module for E-commerce Integration: Direct Interactions Shopify Use Lib Module

These rules are ALWAYS ACTIVE for all code interacting with the Shopify API or related e-commerce functionality.

### Rules

- **R-SHOPIFY-001** MUST: All direct interactions with the Shopify API MUST use the `lib/shopify` module.

### Verify

```bash
# Discover and run the project's integration tests for Shopify API interactions.
# Discover and run the project's unit tests for `lib/shopify` module functions.
# Discover and run the project's end-to-end tests covering cart management flows.
```

**Accept when:**
- All Shopify-related integration tests pass successfully.
- Unit tests for `lib/shopify` functions demonstrate expected behavior.
- End-to-end tests for e-commerce flows complete without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>