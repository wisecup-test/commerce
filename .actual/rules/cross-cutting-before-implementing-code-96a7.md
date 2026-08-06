# Adopt Async Function Exports as Standard Concurrency Model for Server Components and Actions: Before Implementing Code

These rules are ALWAYS ACTIVE for all server-side component rendering, data fetching, and server action implementations.

### Rules

- **R-ASYNC-001** MUST: Before implementing code that depends on versioned framework or library APIs, discover the project's dependency lock artifact, resolve the exact installed version, and verify API compatibility against that version's official documentation.
- **R-ASYNC-002** MUST: All server component exports that perform I/O operations be declared as async functions and properly await asynchronous operations.
- **R-ASYNC-003** MUST: All server action exports that mutate state or call external services be declared as async functions with comprehensive error handling.
- **R-ASYNC-004** MUST: When implementing server actions that mutate cache layers, ensure all cache operations are awaited before proceeding with subsequent logic to prevent race conditions between cache writes and redirects.
- **R-ASYNC-005** MUST: Implement error logging at async function boundaries using the established logging mechanism to capture failures in external API calls, cache operations, and file system access.
- **R-ASYNC-006** SHOULD: For server components that pass Promise objects to context providers, document the deferred resolution pattern and verify that downstream consumers properly handle the Promise lifecycle.
- **R-ASYNC-007** SHOULD: Document patterns for parallel execution using Promise.all() when operations are independent, and profile server response times to identify serialization bottlenecks.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite
# that validates server component rendering and server action behavior

# Discover the project's static analysis tooling and run type checking
# to verify all async function exports have proper return type annotations

# Discover the project's linting configuration and execute linting rules
# that enforce await usage for Promise-returning operations
```

**Accept when:**
- All server component exports that perform I/O operations are declared as async functions and properly await asynchronous operations
- All server action exports that mutate state or call external services are declared as async functions with comprehensive error handling
- Type checking passes without errors related to Promise handling or async function return types
- All cache layer mutations are awaited before proceeding with subsequent logic
- Error logging is implemented at async function boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block implementation. Code review MUST validate that all server-side I/O operations use async/await patterns. Integration tests MUST verify proper execution order and error handling in async server actions.
</enforcement>