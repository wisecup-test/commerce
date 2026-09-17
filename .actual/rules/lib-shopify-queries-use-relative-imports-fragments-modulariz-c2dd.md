# Modularization of Query Fragments within `lib/shopify`: Use Relative Imports Fragments Modularizing Reusable

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: Use relative imports to `../fragments/...` for modularizing reusable query parts or data structures within the `lib/shopify` module.

### Verify

```bash
# Discover the project's linting configuration and execute the linter across the `lib/shopify` module.
# Discover the project's test suite and execute all tests related to `lib/shopify` queries.
# Discover the project's build command and execute a full build of the application.
```

**Accept when:**
- The linter reports no errors or warnings related to fragment imports or definitions.
- All tests within the `lib/shopify` module pass successfully.
- The application builds without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>