# Adoption of lib/shopify and lib/shopify/types Modules: Lib Shopify Types Module Used Data

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: The `lib/shopify/types` module MUST be used for all data type definitions related to Shopify entities.

### Verify

```bash
# Inspect the project's dependency manifest to identify declared library ranges.
# Examine the project's lock file to determine the exact resolved versions of core libraries.
# Run the project's test suite to ensure existing integrations with `lib/shopify` and `lib/shopify/types` function correctly.
```

**Accept when:**
- The project's build process completes without errors related to Shopify integration.
- All unit and integration tests for components using `lib/shopify` and `lib/shopify/types` pass.
- Code reviews confirm adherence to the prescribed usage patterns for these modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>