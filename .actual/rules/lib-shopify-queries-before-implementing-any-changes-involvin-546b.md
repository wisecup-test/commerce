# Modular Query Definitions Using Local Fragments: Before Implementing Any Changes Involving Versioned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MQD-001** MUST: Before implementing any changes involving versioned dependencies, developers MUST discover the project's dependency lock file and verify the exact resolved version of all relevant packages.

### Verify

```bash
# Discover the project's linting configuration and run the linter across the `lib/shopify/queries` directory.
# Discover the project's test runner and execute relevant unit or integration tests for `lib/shopify/queries`.
# Discover the project's build command and perform a full build to ensure no compilation errors related to fragment imports.
```

**Accept when:**
- Linter reports no errors related to fragment imports or structure.
- All relevant tests for queries using fragments pass successfully.
- The project builds without errors, and the application functions as expected with modularized queries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>