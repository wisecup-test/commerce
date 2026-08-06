# Adopt Async Function Declarations for Server-Side Data Operations: Page Rendering Functions

These rules are ALWAYS ACTIVE for all server-side data operations, API route handlers, and component rendering functions that perform I/O operations.

### Rules

- **R-ASYNC-001** MUST: Page rendering functions, metadata generation functions, and image generation functions that fetch data MUST be declared as async functions.
- **R-ASYNC-002** MUST: Server-side API route handlers that perform I/O operations MUST be declared as async functions.
- **R-ASYNC-003** MUST: Server component rendering functions that fetch data MUST be declared as async functions.
- **R-ASYNC-004** MUST: Data fetching functions that call external services MUST be declared as async functions.
- **R-ASYNC-005** MUST: Cart management actions that perform I/O MUST be declared as async functions.
- **R-ASYNC-006** MUST: Functions that access cookies, headers, or request context MUST be declared as async functions.
- **R-ASYNC-007** MUST: File system operations for font loading or asset access MUST be declared as async functions.
- **R-ASYNC-008** SHOULD: For functions that initiate multiple independent async operations, use parallel execution patterns (Promise.all or Promise.allSettled) to minimize total latency rather than awaiting each operation sequentially.
- **R-ASYNC-009** MUST: All async functions MUST have appropriate error handling using try-catch blocks or Promise rejection handlers, with sufficient context for debugging.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting tool
# to verify all I/O functions are declared as async
# (Tool and configuration location to be discovered from project repository)

# Discover the project's test suite and execute tests that validate async function
# behavior, error handling, and Promise resolution
# (Test runner and suite location to be discovered from project repository)

# Discover the project's type checking configuration and execute the type checker
# to verify all async functions return Promise types
# (Type checker and configuration location to be discovered from project repository)
```

**Accept when:**
- All server-side functions that perform I/O operations are declared as async and return Promises
- Static analysis and type checking pass without errors related to async function declarations or Promise handling
- Test suite validates that async operations handle errors correctly and complete successfully
- Code review checklist verification confirms async/await usage for all I/O operations
- No unhandled Promise rejections are detected in runtime monitoring

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review must identify and require correction of synchronous I/O operations. Runtime monitoring must alert on unhandled Promise rejections. Exceptions require architectural review and documentation with technical lead sign-off.
</enforcement>