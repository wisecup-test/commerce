# Adopt Async Function Declarations for Server-Side Data Operations: Functions Pass Promise

These rules are ALWAYS ACTIVE for all server-side data operations, API route handlers, and component rendering functions that perform I/O operations.

### Rules

- **R-ASYNC-001** MUST: Declare all server-side functions that perform I/O operations (external API calls, file system access, cookie/header inspection, database queries) as async functions.
- **R-ASYNC-002** MAY: Pass Promise objects to child components or context providers without awaiting them, allowing the consumer to control resolution timing.
- **R-ASYNC-003** SHOULD: Use parallel execution patterns (Promise.all or Promise.allSettled) for multiple independent async operations to minimize total latency rather than awaiting each operation sequentially.
- **R-ASYNC-004** MUST: Implement appropriate error handling using try-catch blocks or Promise rejection handlers for all async functions, with sufficient context for debugging.
- **R-ASYNC-005** MUST: Ensure all async functions return Promise types as verified by type checking.

### Verify

```bash
# Discover and run the project's static analysis tool to verify all I/O functions are declared as async
# (Tool and configuration location must be discovered from the project repository)

# Discover and run the project's type checker to verify all async functions return Promise types
# (Type checker configuration must be discovered from the project repository)

# Discover and run the project's test suite to validate async function behavior, error handling, and Promise resolution
# (Test runner and configuration must be discovered from the project repository)
```

**Accept when:**
- All server-side functions that perform I/O operations are declared as async and return Promises
- Static analysis and type checking pass without errors related to async function declarations or Promise handling
- Test suite validates that async operations handle errors correctly and complete successfully
- Code review confirms async/await usage for all I/O operations follows the parallel execution pattern guidance

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures, type checking errors, or test failures block acceptance of code changes.
</enforcement>