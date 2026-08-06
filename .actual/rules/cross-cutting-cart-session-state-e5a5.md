# Use Server-Side Cookie Storage for Cart Session State: Cart Session State

These rules are ALWAYS ACTIVE for all server-side action handlers that manage cart operations, including add, remove, update, checkout, and cart creation workflows that require session state persistence across requests.

### Rules

- **R-CART-001** MUST: Cart session state persistence MUST use server-side cookie storage accessed through the runtime's async cookie API.
- **R-CART-002** MUST: All cookie write operations MUST be wrapped in try-catch blocks with console error logging to ensure cart operation failures are observable in server logs.
- **R-CART-003** MUST: Cart action handlers MUST use async function declarations when accessing the async cookie API.
- **R-CART-004** SHOULD: Establish cookie naming conventions and expiration policies that align with cart session lifecycle requirements and coordinate with any existing session management patterns in the codebase.
- **R-CART-005** SHOULD: Implement transaction-like semantics that roll back cart operations on cookie write failures to maintain consistency.

### Verify

```bash
# Discover the project's test suite location and execute server-side action tests
# that validate cart identifier persistence across simulated requests.
find . -type f -name '*.test.*' -o -name '*.spec.*' | grep -i cart | head -5

# Locate the project's linting or static analysis configuration and run checks
# that enforce async function declarations for cart action handlers.
find . -type f \( -name '.eslintrc*' -o -name 'tsconfig.json' -o -name '.prettierrc*' \) | head -3

# Identify the project's integration test framework and run tests that verify
# cookie storage behavior under error conditions and API failures.
grep -r "describe\|test\|it(" . --include="*.test.*" --include="*.spec.*" | grep -i cookie | head -5
```

**Accept when:**
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.
- Code review verification confirms async cookie API usage patterns and error handling are present in all cart action handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart action handlers MUST demonstrate cookie persistence behavior through passing test suites before code review approval.
</enforcement>