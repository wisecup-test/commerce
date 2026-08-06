# Use Server-Side Cookie Storage for Cart Session State: Cart Identifier Writes

These rules are ALWAYS ACTIVE for all server-side action handlers that perform cart operations including add, remove, update, checkout, and cart creation workflows that establish new session state.

### Rules

- **R-CART-001** MUST: Cart identifier writes MUST occur after successful cart creation or modification operations complete.
- **R-CART-002** MUST: All cookie write operations MUST be wrapped in try-catch blocks with console error logging to ensure cart operation failures are observable in server logs.
- **R-CART-003** MUST: Cart action handlers MUST use async function declarations when accessing server-side cookie APIs.
- **R-CART-004** SHOULD: Establish cookie naming conventions and expiration policies that align with cart session lifecycle requirements and coordinate with any existing session management patterns in the codebase.

### Verify

```bash
# Discover the project's test suite location and execute server-side action tests
# that validate cart identifier persistence across simulated requests.
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i cart | head -5

# Locate the project's linting or static analysis configuration and run checks
# that enforce async function declarations for cart action handlers.
find . -type f \( -name ".eslintrc*" -o -name "eslint.config.*" -o -name "tsconfig.json" \) | head -3

# Identify the project's integration test framework and run tests that verify
# cookie storage behavior under error conditions and API failures.
grep -r "describe\|test\|it(" --include="*.test.*" --include="*.spec.*" | grep -i cookie | head -5
```

**Accept when:**
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.
- All cart action handlers use async/await syntax when calling server-side cookie APIs.
- Try-catch blocks wrap all cookie write operations with console.error logging.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for cart operation implementations.
</enforcement>