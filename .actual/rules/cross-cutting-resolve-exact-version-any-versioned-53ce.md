# Adopt Internal Shopify Integration Library: Resolve Exact Version Any Versioned Dependency

These rules are ALWAYS ACTIVE for all code modules or components that interact with the Shopify platform.

### Rules

- **R-SHOPIFY-001** MUST: Resolve the exact version of any versioned dependency used by the internal Shopify integration library by inspecting the project's lock file before implementation.

### Verify

```bash
# Inspect the project's dependency manifest for declarations of Shopify-related packages.
# Examine the project's build configuration to identify how modules are resolved and bundled.
# Search the codebase for imports of the internal Shopify integration library to confirm its usage.
```

**Accept when:**
- All new features requiring Shopify integration utilize the internal Shopify integration library.
- No direct calls to external Shopify APIs are found outside the designated integration library.
- The internal library provides a consistent interface for common Shopify operations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>