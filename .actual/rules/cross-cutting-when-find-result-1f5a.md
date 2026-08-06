# Adopt Array.find() for Predicate-Based Collection Queries: When Find Result

These rules are ALWAYS ACTIVE for all in-memory array and collection queries within server actions, page rendering logic, cart line item queries by merchandise identifier, and sort configuration lookups by slug parameter.

### Rules

- **R-FIND-001** SHOULD: When the find result is used in a conditional context, use nullish coalescing (`??`) or logical OR (`||`) operators to provide fallback values instead of separate conditional blocks.
- **R-FIND-002** MUST: Use `Array.find()` with arrow function predicates for single-element retrieval from in-memory collections instead of imperative for-loops with early return or break statements.
- **R-FIND-003** MUST: Handle the undefined return value from `find()` explicitly; do not assume a match will always be found.
- **R-FIND-004** MUST: Ensure predicate functions passed to `find()` are pure and free of side effects.
- **R-FIND-005** SHOULD: Enable strict null checking in the type system to catch missing undefined handling at compile time.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting verification script
# to ensure all collection queries use find instead of imperative loops
lint:verify

# Locate the project's type checking configuration and run the type verification script
# with strict null checks enabled to confirm undefined handling is present
type:check --strict

# Identify the project's test suite entry point and execute unit tests covering
# cart actions and search page logic to verify find operations handle both match and no-match cases
test:run --grep "find|cart|search"
```

**Accept when:**
- All predicate-based single-element queries in cart actions and search logic use `find()` with arrow function predicates
- Type checking passes with strict null checks enabled and all `find()` results are handled for undefined cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios
- No imperative loop patterns remain where `find()` is applicable

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review approval.
</enforcement>