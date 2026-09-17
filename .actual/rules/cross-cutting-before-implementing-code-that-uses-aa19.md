# Adopt and Standardize Shopify Storefront Interaction via Internal Module: Before Implementing Code That Uses Versioned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VERSIONED-001** MUST: Before implementing code that uses a versioned library, the consumer MUST discover the ecosystem's lock file and resolve the exact locked version.

### Verify

```bash
# Inspect the project's dependency manifest for modules related to external API interactions.
# Examine relevant modules for calls to external services and environment variable usage.
# Review the project's test suite for coverage of Shopify integration points.
```

**Accept when:**
- All Shopify API interactions are routed through the designated internal module.
- Sensitive credentials are sourced exclusively from environment variables.
- Data caching mechanisms are correctly applied to storefront data, and revalidation triggers are functional.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>