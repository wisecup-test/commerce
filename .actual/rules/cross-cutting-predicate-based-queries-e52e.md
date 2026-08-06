# Adopt Array.find() for Predicate-Based Collection Queries: Predicate Based Queries

These rules are ALWAYS ACTIVE for all predicate-based single-element queries against in-memory collections in server actions, page rendering logic, cart operations, and configuration lookups.

### Rules

- **R-PRED-001** MUST: All predicate-based queries for a single element in an in-memory collection must use the native find method with an inline predicate function.
- **R-PRED-002** MUST: When migrating existing imperative loops to find, verify that the original loop did not rely on index position or mutation of external state during iteration.
- **R-PRED-003** MUST: For collections where no match is expected to be a valid business case, use nullish coalescing or logical OR to provide default values inline rather than separate conditional blocks.
- **R-PRED-004** SHOULD: Enable strict null checking in the type system to catch missing undefined handling at compile time rather than runtime.
- **R-PRED-005** SHOULD: Document that find short-circuits on first match and prohibit side effects in predicate functions through linting rules.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting verification script
# to ensure all collection queries use find instead of imperative loops
lint:verify-find-usage

# Locate the project's type checking configuration and run the type verification script
# with strict null checks enabled to confirm undefined handling is present
type:verify-strict-null-checks

# Identify the project's test suite entry point and execute unit tests covering
# cart actions and search page logic to verify find operations handle both match and no-match cases
test:verify-predicate-queries
```

**Accept when:**
- All predicate-based single-element queries in cart actions and search logic use find with arrow function predicates
- Type checking passes with strict null checks enabled and all find results are handled for undefined cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios
- Linting verification confirms no imperative loop patterns remain where find is applicable

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification commands must pass before accepting changes that introduce or modify predicate-based collection queries. Type errors from missing undefined handling block build pipeline progression. Linting failures block merge to main branch until imperative loops are refactored to find.
</enforcement>