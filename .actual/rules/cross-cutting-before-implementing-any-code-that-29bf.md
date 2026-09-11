# Adoption of `lib/shopify` for E-commerce Integration: Before Implementing Any Code That Uses

These rules are ALWAYS ACTIVE for any code interacting with versioned libraries, particularly when integrating with the Shopify platform via `lib/shopify`.

### Rules

- **R-SHOPIFY-001** MUST: Before implementing any code that uses a versioned library, the consumer MUST discover the ecosystem's lock file and resolve the exact locked version.
- **R-SHOPIFY-002** MUST: Find the dependency manifest in the repo. It declares ranges, not installed versions.
- **R-SHOPIFY-003** MUST: Identify the build tool from the manifest.
- **R-SHOPIFY-004** MUST: Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
- **R-SHOPIFY-005** MUST: Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
- **R-SHOPIFY-006** MUST: Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
- **R-SHOPIFY-007** MUST: For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

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