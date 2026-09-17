# Adopt and Standardize Shopify Storefront Interaction via Internal Module: Error Logging External Interactions Use Standard

These rules are ALWAYS ACTIVE for modules responsible for interacting with the Shopify storefront API, components managing cart state and product data, and any code path requiring secure access to external service credentials.

### Rules

- **R-SHOPIFY-001** SHOULD: Error logging for external API interactions SHOULD use the standard application logging mechanism.

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