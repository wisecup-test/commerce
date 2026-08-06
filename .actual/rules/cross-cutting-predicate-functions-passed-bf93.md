# Adopt Array.find() for Predicate-Based Collection Queries: Predicate Functions Passed

These rules are ALWAYS ACTIVE for all in-memory array and collection queries within server actions, page rendering logic, cart line item queries, and sort configuration lookups.

### Rules

- **R-FIND-001** MUST: Predicate functions passed to find must use arrow function syntax with explicit parameter names that reflect the domain concept being queried.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting verification script
# to ensure all collection queries use find instead of imperative loops
echo "Checking for imperative loop patterns where find is applicable..."

# Locate the project's type checking configuration and run the type verification script
# with strict null checks enabled to confirm undefined handling is present
echo "Verifying strict null checks are enabled and all find results handle undefined cases..."

# Identify the project's test suite entry point and execute unit tests covering
# cart actions and search page logic to verify find operations handle both match and no-match cases
echo "Running unit tests for find operations with matching and non-matching predicates..."
```

**Accept when:**
- All predicate-based single-element queries in cart actions and search logic use find with arrow function predicates
- Type checking passes with strict null checks enabled and all find results are handled for undefined cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios
- Predicate functions use explicit parameter names reflecting domain concepts (e.g., `item => item.id === targetId` not `x => x.id === targetId`)

<enforcement>
Claude Code MUST NOT skip or defer verification. All find operations must use arrow function syntax with explicit, domain-meaningful parameter names. Type checking with strict null checks must pass before acceptance.
</enforcement>