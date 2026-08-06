# Adopt Array.find() for Predicate-Based Collection Queries: Code Handling Result

These rules are ALWAYS ACTIVE for all in-memory array and collection queries within server actions, page rendering logic, cart line item queries by merchandise identifier, and sort configuration lookups by slug parameter.

### Rules

- **R-FIND-001** MUST: Code handling the result of a find operation must account for the undefined return value when no element matches the predicate.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting verification script
# to ensure all collection queries use find instead of imperative loops
echo "Checking for imperative loop patterns where find is applicable..."

# Locate the project's type checking configuration and run the type verification script
# with strict null checks enabled to confirm undefined handling is present
echo "Running type checking with strict null checks enabled..."

# Identify the project's test suite entry point and execute unit tests covering
# cart actions and search page logic to verify find operations handle both match and no-match cases
echo "Executing unit tests for find operations with match and no-match scenarios..."
```

**Accept when:**
- All predicate-based single-element queries in cart actions and search logic use find with arrow function predicates
- Type checking passes with strict null checks enabled and all find results are handled for undefined cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification steps (linting, type checking with strict null checks, and unit test execution) are mandatory before accepting changes that introduce or modify find operations.
</enforcement>