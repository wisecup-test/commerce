# Adopt Async Function Declarations for Server-Side Data Operations: Cart Management Operations

These rules are ALWAYS ACTIVE for all server-side data operations, API route handlers, and component rendering functions that perform I/O operations.

### Rules

- **R-CART-001** MUST: Cart management operations that modify state or trigger side effects MUST be implemented as async functions.
- **R-CART-002** MUST: Server-side API route handlers MUST be declared as async functions.
- **R-CART-003** MUST: Server component rendering functions that perform I/O MUST be declared as async functions.
- **R-CART-004** MUST: Data fetching functions that call external services MUST be declared as async functions.
- **R-CART-005** MUST: Metadata and Open Graph image generation functions MUST be declared as async functions.
- **R-CART-006** MUST: Functions that access cookies, headers, or request context MUST be declared as async functions.
- **R-CART-007** MUST: File system operations for font loading or asset access MUST be declared as async functions.
- **R-CART-008** MUST: All async functions MUST have appropriate error handling using try-catch blocks or Promise rejection handlers.
- **R-CART-009** SHOULD: For functions that initiate multiple independent async operations, use parallel execution patterns (Promise.all or Promise.allSettled) to minimize total latency rather than awaiting each operation sequentially.
- **R-CART-010** SHOULD: Errors in async functions SHOULD be logged with sufficient context for debugging.
- **R-CART-011** MAY: Long-running async operations MAY implement timeout mechanisms for external API calls and ensure cleanup logic runs in finally blocks.

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
- All async functions have appropriate error handling using try-catch blocks or Promise rejection handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review must identify and require correction of synchronous I/O operations. Runtime monitoring must alert on unhandled Promise rejections.
</enforcement>