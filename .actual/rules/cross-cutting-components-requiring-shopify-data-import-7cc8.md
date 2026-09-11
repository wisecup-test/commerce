# Adoption of `lib/shopify` for E-commerce Integration: Components Requiring Shopify Data Import Directly

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** SHOULD: Components requiring Shopify data SHOULD import directly from `lib/shopify` or its sub-modules (e.g., `lib/shopify/types`).

### Verify

```bash
# Inspect the project's dependency manifest to identify the `lib/shopify` module.
# (This step requires knowledge of the project's specific dependency management tools, e.g., package.json, Cargo.toml, etc.)

# Search the codebase for imports of `lib/shopify` and its sub-modules.
# Example for JavaScript/TypeScript projects:
# grep -r "import .* from 'lib/shopify'" . --exclude-dir=node_modules
# grep -r "require('lib/shopify')" . --exclude-dir=node_modules

# Review API routes and UI components to confirm all Shopify interactions use `lib/shopify`.
# This often involves manual code review or more advanced static analysis.
# A basic check might involve looking for direct Shopify API calls outside the `lib/shopify` directory:
# grep -r "shopify.com/admin/api" . --exclude-dir=lib/shopify --exclude-dir=node_modules
```

**Accept when:**
- All Shopify-related imports originate from `lib/shopify` or its sub-modules.
- No direct calls to Shopify APIs are found outside of `lib/shopify`.
- The `lib/shopify` module is well-structured and documented.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>