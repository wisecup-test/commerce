# Adopt Internal Shopify Integration Library: Extend Internal Shopify Integration Library New

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MAY: Extend the internal Shopify integration library with new functionalities as required by application features.

### Verify

```bash
Inspect the project's dependency manifest for declarations of Shopify-related packages.
Examine the project's build configuration to identify how modules are resolved and bundled.
Search the codebase for imports of the internal Shopify integration library to confirm its usage.
```

**Accept when:**
- All new features requiring Shopify integration utilize the internal Shopify integration library.
- No direct calls to external Shopify APIs are found outside the designated integration library.
- The internal library provides a consistent interface for common Shopify operations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>