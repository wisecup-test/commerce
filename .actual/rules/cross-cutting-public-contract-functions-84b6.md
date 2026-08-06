# Adopt Async Function Exports for Concurrent Server Action Composition: Public Contract Functions

These rules are ALWAYS ACTIVE for all server action modules that export functions for external consumption, particularly those performing I/O operations such as external HTTP requests, cache mutations, or request-scoped context access (cookies, headers, search parameters).

### Rules

- **R-ASYNC-001** MUST: All public API contract functions that perform I/O operations (external HTTP requests, cache mutations, cookie/header access) must be exported as async functions.
- **R-ASYNC-002** MUST: Wrap the entire function body in a try-catch block when the function coordinates external service calls, and log caught errors to console.error with sufficient context (operation name, input parameters).
- **R-ASYNC-003** MUST: Await all I/O operations (fetch, cache access, cookie/header reads) before proceeding to dependent operations.
- **R-ASYNC-004** MUST: For operations that mutate server-side state (cache writes, cookie sets), await the mutation before returning or proceeding to subsequent operations to ensure state consistency.
- **R-ASYNC-005** SHOULD: Group related async operations into cohesive modules with clear export boundaries and use descriptive function names that indicate the async nature and side effects of each operation.

### Verify

```bash
# Discover the project's module analysis tooling and execute static analysis to identify all exported functions in server action modules, verifying that functions performing I/O are declared as async.
# (Tool and command to be derived from project repository)

# Locate the project's linting configuration and run the linter to detect missing await keywords or unhandled promise rejections in async functions.
# (Linting tool and command to be derived from project repository)

# Identify the project's test suite and execute integration tests that verify error logging behavior for async operations under failure conditions.
# (Test runner and command to be derived from project repository)
```

**Accept when:**
- All exported functions performing I/O operations (external HTTP, cache mutations, request context access) are declared as async and use await at each async boundary.
- Static analysis confirms no missing await keywords or unhandled promise rejections in async server action functions.
- Integration tests demonstrate that errors in async operations are logged to console.error with sufficient context for debugging.
- All async functions that coordinate external services include try-catch blocks with error logging.
- All state mutations are awaited before function return or proceeding to subsequent operations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before approving changes to server action modules.
</enforcement>