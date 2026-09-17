# Modular Query Definitions Using Local Fragments: Query Definitions Within Lib Shopify Queries

These rules are ALWAYS ACTIVE for all files defining data fetching queries within the `lib/shopify/queries` directory.

### Rules

- **R-MODULAR-QUERY-001** MUST: Query definitions within the `lib/shopify/queries` directory MUST modularize reusable data structures or sub-queries into dedicated local fragment files.

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