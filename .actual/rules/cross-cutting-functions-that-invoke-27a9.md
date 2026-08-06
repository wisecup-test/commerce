# Adopt Async Function Declarations for Server-Side Data Operations: Functions That Invoke

These rules are ALWAYS ACTIVE for all server-side data operations, API route handlers, and component rendering functions that perform I/O operations.

### Rules

- **R-ASYNC-001** MUST: Functions that invoke external HTTP APIs, read from the file system, or access runtime request context MUST use await to unwrap Promise results.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting tool to verify all I/O functions are declared as async
# Discover the project's test suite and execute tests that validate async function behavior, error handling, and Promise resolution
# Discover the project's type checking configuration and execute the type checker to verify all async functions return Promise types
```

**Accept when:**
- All server-side functions that perform I/O operations are declared as async and return Promises
- Static analysis and type checking pass without errors related to async function declarations or Promise handling
- Test suite validates that async operations handle errors correctly and complete successfully

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review identifies and requires correction of synchronous I/O operations. Runtime monitoring alerts on unhandled Promise rejections.
</enforcement>