# Adoption and Usage Pattern of `lib/shopify` Module for E-commerce Integration: Shopify Credentials Revalidation Secrets Managed Exclusively

These rules are ALWAYS ACTIVE for all code interacting with the Shopify API, modules managing e-commerce cart state, and server-side actions requiring revalidation secrets.

### Rules

- **R-SHOPIFY-001** MUST: Shopify API credentials and revalidation secrets MUST be managed exclusively through environment variables.

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