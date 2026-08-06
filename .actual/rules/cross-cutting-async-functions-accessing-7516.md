# Adopt Async Function Exports for Concurrent Server Action Composition: Async Functions Accessing

These rules are ALWAYS ACTIVE for server action modules that export functions for external consumption, functions performing HTTP requests to external APIs, operations mutating server-side cache or revalidation state, and functions accessing request-scoped context (cookies, headers, search parameters).

### Rules

- **R-ASYNC-001** SHOULD: Async functions accessing request-scoped context (cookies, headers) should await the context accessor before dereferencing properties to prevent race conditions.
- **R-ASYNC-002** MUST: All exported functions performing I/O operations (external HTTP, cache mutations, request context access) must be declared as async and use await at each async boundary.
- **R-ASYNC-003** MUST: Wrap the entire function body in a try-catch block when the function coordinates external service calls, and log caught errors to console.error with sufficient context (operation name, input parameters).
- **R-ASYNC-004** MUST: For operations that mutate server-side state (cache writes, cookie sets), await the mutation before returning or proceeding to subsequent operations to ensure state consistency.
- **R-ASYNC-005** SHOULD: Group related async operations into cohesive modules with clear export boundaries and use descriptive function names that indicate the async nature and side effects of each operation.

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
- All async functions that coordinate external services include try-catch blocks with error logging.
- All mutations to server-side state (cache writes, cookie sets) are awaited before function return or proceeding to subsequent operations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools must scan for async function declarations and await keyword usage. Code review must verify error handling in all async functions. Linting failures block pull request merges until async/await patterns are corrected.
</enforcement>