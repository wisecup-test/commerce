# Adoption of `lib/shopify` for Shopify Storefront Integration: Project Utilize Next Cache Headers Data

These rules are ALWAYS ACTIVE for all modules and components requiring interaction with the Shopify Storefront API.

### Rules

- **R-SHOPIFY-001** SHOULD: The project SHOULD utilize `next/cache` and `next/headers` for data caching and header management within Shopify API interactions.

### Verify

```bash
Discover and execute the project's integration tests for Shopify API interactions.
Discover and execute the project's end-to-end tests involving cart functionality.
Discover and execute the project's linting and static analysis checks.
```

**Accept when:**
- All Shopify integration tests pass without errors.
- Cart-related end-to-end flows function correctly, demonstrating proper data fetching and caching.
- No linting or static analysis warnings related to Shopify integration are reported.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>