# Adopt Async Function Exports for Concurrent Server Action Composition: Modules Export Both

These rules are ALWAYS ACTIVE for server action modules that export functions for external consumption, particularly those performing HTTP requests to external APIs, mutating server-side cache or revalidation state, or accessing request-scoped context (cookies, headers, search parameters).

### Rules

- **R-ASYNC-001** MAY: Modules may export both high-level async orchestration functions and lower-level async primitives to support composition at multiple abstraction levels.
- **R-ASYNC-002** MUST: All exported functions performing I/O operations (external HTTP calls, cache mutations, request context access) shall be declared as async.
- **R-ASYNC-003** MUST: All async I/O operations within async functions shall use await at each async boundary before proceeding to dependent operations.
- **R-ASYNC-004** MUST: Functions coordinating external service calls shall wrap the entire function body in a try-catch block and log caught errors to console.error with sufficient context (operation name, input parameters).
- **R-ASYNC-005** MUST: Operations mutating server-side state (cache writes, cookie sets) shall await the mutation before returning or proceeding to subsequent operations to ensure state consistency.
- **R-ASYNC-006** SHOULD: Group related async operations into cohesive modules with clear export boundaries and use descriptive function names that indicate the async nature and side effects of each operation.

### Verify

```bash
# Discover the project's module analysis tooling and execute static analysis
# to identify all exported functions in server action modules, verifying that
# functions performing I/O are declared as async.

# Locate the project's linting configuration and run the linter to detect
# missing await keywords or unhandled promise rejections in async functions.

# Identify the project's test suite and execute integration tests that verify
# error logging behavior for async operations under failure conditions.
```

**Accept when:**
- All exported functions performing I/O operations (external HTTP, cache mutations, request context access) are declared as async and use await at each async boundary.
- Static analysis confirms no missing await keywords or unhandled promise rejections in async server action functions.
- Integration tests demonstrate that errors in async operations are logged to console.error with sufficient context for debugging.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools must scan for async function declarations and await keyword usage. Code review must verify error handling (try-catch blocks) in all async functions coordinating external services. Continuous integration must run linting rules enforcing async/await patterns and detecting unhandled promise rejections. Linting failures block pull request merges. Code review identifies missing error handling or incorrect await usage and requests changes before approval. Runtime monitoring alerts on unhandled promise rejections in production.
</enforcement>