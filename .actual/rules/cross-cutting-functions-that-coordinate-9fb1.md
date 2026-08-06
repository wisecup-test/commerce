# Adopt Async Function Exports for Concurrent Server Action Composition: Functions That Coordinate

These rules are ALWAYS ACTIVE for server action modules that export functions for external consumption, particularly those performing HTTP requests to external APIs, mutating server-side cache or revalidation state, or accessing request-scoped context (cookies, headers, search parameters).

### Rules

- **R-ASYNC-001** MUST: Functions that coordinate multiple async operations (cache writes, external fetches, state mutations) must use await at each async boundary to enforce sequential consistency where order matters.
- **R-ASYNC-002** MUST: All exported functions performing I/O operations (external HTTP, cache mutations, request context access) must be declared as async.
- **R-ASYNC-003** MUST: Wrap the entire function body in a try-catch block when the function coordinates external service calls.
- **R-ASYNC-004** MUST: Log caught errors to console.error with sufficient context (operation name, input parameters) to enable debugging.
- **R-ASYNC-005** MUST: For operations that mutate server-side state (cache writes, cookie sets), await the mutation before returning or proceeding to subsequent operations to ensure state consistency.
- **R-ASYNC-006** SHOULD: Group related async operations into cohesive modules with clear export boundaries and use descriptive function names that indicate the async nature and side effects of each operation.

### Verify

```bash
# Discover the project's module analysis tooling and execute static analysis to identify all exported functions in server action modules, verifying that functions performing I/O are declared as async.
# Locate the project's linting configuration and run the linter to detect missing await keywords or unhandled promise rejections in async functions.
# Identify the project's test suite and execute integration tests that verify error logging behavior for async operations under failure conditions.
```

**Accept when:**
- All exported functions performing I/O operations (external HTTP, cache mutations, request context access) are declared as async and use await at each async boundary.
- Static analysis confirms no missing await keywords or unhandled promise rejections in async server action functions.
- Integration tests demonstrate that errors in async operations are logged to console.error with sufficient context for debugging.
- Code review verification confirms error handling (try-catch blocks) in all async functions that coordinate external services.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan for async function declarations and await keyword usage. Linting rules MUST enforce async/await patterns and detect unhandled promise rejections. Violations block pull request merges until patterns are corrected.
</enforcement>