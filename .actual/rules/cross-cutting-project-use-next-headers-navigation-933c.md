# Adoption and Usage Pattern of `lib/shopify` Module for E-commerce Integration: Project Use Next Headers Navigation Accessing

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** SHOULD: The project SHOULD use `next/headers` and `next/navigation` for accessing request-specific context and managing redirects within Shopify-related actions.

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