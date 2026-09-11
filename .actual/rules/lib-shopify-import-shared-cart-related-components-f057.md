# Modular Cart Fragments in Shopify Integration: Import Shared Cart Related Components Fragments

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CART-FRAG-001** MUST: Import shared cart-related components from the `fragments/cart` module using relative paths within the `lib/shopify` module.

### Verify

```bash
# Inspect the dependency graph for `lib/shopify/mutations/cart.ts` to confirm imports from `fragments/cart`.
# Inspect the dependency graph for `lib/shopify/queries/cart.ts` to confirm imports from `fragments/cart`.
# Run static analysis tools to detect unreferenced or duplicated cart-related logic outside `fragments/cart` within `lib/shopify`.
```

**Accept when:**
- `lib/shopify/mutations/cart.ts` and `lib/shopify/queries/cart.ts` exclusively import shared cart components from `fragments/cart`.
- No duplicated cart-related types or utility functions are found within `lib/shopify` outside of `fragments/cart`.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>