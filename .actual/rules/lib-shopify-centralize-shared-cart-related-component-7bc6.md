# Modular Cart Fragments in Shopify Integration: Centralize Shared Cart Related Components Within

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CART-001** MUST: Centralize shared cart-related components within the `fragments/cart` module for use by `lib/shopify` components.

### Verify

```bash
Inspect the dependency graph for `lib/shopify/mutations/cart.ts` to confirm imports from `fragments/cart`.
Inspect the dependency graph for `lib/shopify/queries/cart.ts` to confirm imports from `fragments/cart`.
Run static analysis tools to detect unreferenced or duplicated cart-related logic outside `fragments/cart` within `lib/shopify`.
```

**Accept when:**
- `lib/shopify/mutations/cart.ts` and `lib/shopify/queries/cart.ts` exclusively import shared cart components from `fragments/cart`.
- No duplicated cart-related types or utility functions are found within `lib/shopify` outside of `fragments/cart`.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>