# Modularization of Core Domain Fragments within `lib/shopify`: Discover Ecosystem Lock File Resolve Exact

These rules are ALWAYS ACTIVE for all modules within the `lib/shopify` directory that define or consume core domain entities.

### Rules

- **R-ADR-001** MUST: Discover the ecosystem's lock file and resolve the exact locked version of any versioned dependency before implementation.

### Verify

```bash
# Inspect the module import graph for `lib/shopify` to confirm adherence to the `fragments` module pattern.
# Run static analysis tools configured to detect unapproved import patterns within the `lib/shopify` directory.
# Execute the project's test suite to ensure no regressions from refactoring or new implementations following this pattern.
```

**Accept when:**
- New domain entity definitions are consistently placed in dedicated `fragments` modules.
- All consuming modules correctly import from these `fragments` modules.
- No circular dependencies are introduced by the modularization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>