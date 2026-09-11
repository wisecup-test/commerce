# Modularization of Core Domain Fragments within `lib/shopify`: Ensure That Fragments Modules Contain Only

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FRAG-001** SHOULD: Ensure that `fragments` modules contain only definitions intended for reuse and do not include operational logic.

### Verify

```bash
# Inspect the module import graph for lib/shopify to confirm adherence to the `fragments` module pattern.
# Run static analysis tools configured to detect unapproved import patterns within the lib/shopify directory.
# Execute the project's test suite to ensure no regressions from refactoring or new implementations following this pattern.
```

**Accept when:**
- New domain entity definitions are consistently placed in dedicated `fragments` modules.
- All consuming modules correctly import from these `fragments` modules.
- No circular dependencies are introduced by the modularization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>