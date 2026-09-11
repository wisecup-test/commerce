# Modularization of Core Domain Fragments within `lib/shopify`: Import Fragments Modules Relative Paths Consuming

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: Import `fragments` modules using relative paths from consuming modules within the same logical domain.

### Verify

```bash
# Inspect the module import graph for `lib/shopify` to confirm adherence to the `fragments` module pattern.
# (Specific command to be derived from project's build/analysis tools, e.g., dependency graph visualization or linter checks)

# Run static analysis tools configured to detect unapproved import patterns within the `lib/shopify` directory.
# (Specific command to be derived from project's static analysis setup, e.g., ESLint with custom rules)

# Execute the project's test suite to ensure no regressions from refactoring or new implementations following this pattern.
# (Specific command to be derived from project's test runner, e.g., `npm test` or `yarn test`)
```

**Accept when:**
- New domain entity definitions are consistently placed in dedicated `fragments` modules.
- All consuming modules correctly import from these `fragments` modules.
- No circular dependencies are introduced by the modularization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>