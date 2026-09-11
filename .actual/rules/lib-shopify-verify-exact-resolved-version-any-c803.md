# Modular Cart Fragments in Shopify Integration: Verify Exact Resolved Version Any Versioned

These rules are ALWAYS ACTIVE for code within the `lib/shopify` directory and components implementing or extending Shopify cart functionality.

### Rules

- **R-VER-001** MUST: Verify the exact resolved version of any versioned dependency before implementation by inspecting the project's lock file.

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