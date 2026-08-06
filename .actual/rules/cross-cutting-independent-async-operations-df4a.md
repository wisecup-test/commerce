# Adopt Async Function Declarations for Server-Side Data Operations: Independent Async Operations

These rules are ALWAYS ACTIVE for all server-side data operations, API route handlers, component rendering functions that perform I/O operations, cart management actions, metadata and Open Graph image generation functions, and functions that access cookies, headers, or request context.

### Rules

- **R-ASYNC-001** SHOULD: Independent async operations that do not depend on each other's results SHOULD be initiated in parallel and awaited together to minimize latency.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting tool
# to verify all I/O functions are declared as async

# Discover the project's test suite and execute tests that validate async function
# behavior, error handling, and Promise resolution

# Discover the project's type checking configuration and execute the type checker
# to verify all async functions return Promise types
```

**Accept when:**
- All server-side functions that perform I/O operations are declared as async and return Promises
- Static analysis and type checking pass without errors related to async function declarations or Promise handling
- Test suite validates that async operations handle errors correctly and complete successfully
- Parallel execution patterns are used for independent async operations to minimize total latency
- All async functions have appropriate error handling using try-catch blocks or Promise rejection handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review must identify and require correction of synchronous I/O operations. Runtime monitoring must alert on unhandled Promise rejections.
</enforcement>