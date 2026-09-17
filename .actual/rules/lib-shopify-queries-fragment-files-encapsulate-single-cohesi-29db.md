# Modular Query Definitions Using Local Fragments: Fragment Files Encapsulate Single Cohesive Unit

These rules are ALWAYS ACTIVE for all files defining data fetching queries within the `lib/shopify/queries` directory.

### Rules

- R-MODULAR-FRAG-001 SHOULD: Fragment files SHOULD encapsulate a single, cohesive unit of reusable query logic or data shape.

### Verify

```bash
# Discover the project's linting configuration and run the linter across the `lib/shopify/queries` directory.
# (e.g., `npm run lint lib/shopify/queries` or `yarn lint lib/shopify/queries`)

# Discover the project's test runner and execute relevant unit or integration tests for `lib/shopify/queries`.
# (e.g., `npm test lib/shopify/queries` or `yarn test lib/shopify/queries`)

# Discover the project's build command and perform a full build to ensure no compilation errors related to fragment imports.
# (e.g., `npm run build` or `yarn build`)
```

**Accept when:**
- Linter reports no errors related to fragment imports or structure.
- All relevant tests for queries using fragments pass successfully.
- The project builds without errors, and the application functions as expected with modularized queries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>