# Use Server-Side Cookie Storage for Cart Session State: Cart Action Handlers

These rules are ALWAYS ACTIVE for all server-side cart action handlers that require persistent session state between requests, including add, remove, update, checkout, and cart creation workflows.

### Rules

- **R-CART-001** MUST: All cart action handlers MUST be declared as async functions to support awaited cookie operations.
- **R-CART-002** MUST: Wrap all cookie write operations in try-catch blocks with console error logging to ensure cart operation failures are observable in server logs.
- **R-CART-003** MUST: Locate the async cookie API in the runtime's server-side module exports and confirm the API surface supports both get and set operations with awaitable promises before implementation.
- **R-CART-004** SHOULD: Establish cookie naming conventions and expiration policies that align with cart session lifecycle requirements and coordinate with any existing session management patterns in the codebase.

### Verify

```bash
# Discover the project's test suite location and execute server-side action tests
# that validate cart identifier persistence across simulated requests.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(cart|action)' | head -5

# Locate the project's linting or static analysis configuration and run checks
# that enforce async function declarations for cart action handlers.
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'tsconfig.json' \) | head -3

# Identify the project's integration test framework and run tests that verify
# cookie storage behavior under error conditions and API failures.
grep -r "describe\|test\|it(" . --include="*.test.*" --include="*.spec.*" | grep -i cart | head -5

# Verify async cookie API usage in cart action handlers
grep -r "async.*function\|const.*=.*async" . --include="*.ts" --include="*.js" | grep -i cart | head -10
```

**Accept when:**
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.
- All cart action handlers are declared as async functions with proper try-catch error handling around cookie operations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart action handlers MUST comply with R-CART-001 through R-CART-004 before code review approval.
</enforcement>