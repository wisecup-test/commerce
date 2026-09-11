# Adoption and Usage Pattern of `lib/shopify` Module for E-commerce Integration: Consumer Verify Exact Resolved Version Any

These rules are ALWAYS ACTIVE for all code interacting with the Shopify API, managing e-commerce cart state, or performing server-side actions requiring revalidation secrets.

### Rules

- **R-VERIFY-VERSION-001** MUST: The consumer MUST verify the exact resolved version of any versioned dependency before implementation by inspecting the project's lock file or resolution artifact.

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