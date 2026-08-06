# Adopt Array.find() for Predicate-Based Collection Queries: Implementations Not Use

These rules are ALWAYS ACTIVE for all in-memory collection queries within server actions, page rendering logic, cart line item operations, and sort configuration lookups where a single element matching a predicate must be retrieved.

### Rules

- **R-FIND-001** MUST_NOT: Implementations must not use imperative for-loops or while-loops for single-element predicate queries where `Array.find()` is applicable.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting verification script
# to ensure all collection queries use find instead of imperative loops
echo "Running linting verification for imperative loop detection..."
# (Exact command depends on project's linter configuration)

# Locate the project's type checking configuration and run type verification
# with strict null checks enabled to confirm undefined handling is present
echo "Running type verification with strict null checks..."
# (Exact command depends on project's type checker configuration)

# Identify the project's test suite entry point and execute unit tests
# covering cart actions and search page logic
echo "Running unit tests for find operations..."
# (Exact command depends on project's test runner configuration)
```

**Accept when:**
- All predicate-based single-element queries in cart actions and search logic use `find()` with arrow function predicates
- Type checking passes with strict null checks enabled and all `find()` results are handled for `undefined` cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios
- No imperative loops are detected in applicable collection query contexts

<enforcement>
Claude Code MUST NOT skip or defer verification. Linting failures block merge to main branch until imperative loops are refactored to find. Type errors from missing undefined handling block build pipeline progression. Code review feedback requires revision before approval when find is not used for applicable queries.
</enforcement>