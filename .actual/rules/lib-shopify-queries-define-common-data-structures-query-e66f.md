# Modularization of Query Fragments within `lib/shopify`: Define Common Data Structures Query Selections

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-QUERY-001** SHOULD: SHOULD define common data structures or query selections that are reused across multiple queries as distinct fragment files.

### Verify

```bash
# Discover the project's linting configuration and execute the linter across the `lib/shopify` module.
# (e.g., `npm run lint lib/shopify` or similar, after discovery)

# Discover the project's test suite and execute all tests related to `lib/shopify` queries.
# (e.g., `npm test -- lib/shopify` or similar, after discovery)

# Discover the project's build command and execute a full build of the application.
# (e.g., `npm run build` or similar, after discovery)
```

**Accept when:**
- The linter reports no errors or warnings related to fragment imports or definitions.
- All tests within the `lib/shopify` module pass successfully.
- The application builds without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>