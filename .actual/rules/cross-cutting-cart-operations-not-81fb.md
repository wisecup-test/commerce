# Use Server-Side Cookie Storage for Cart Session State: Cart Operations Not

These rules are ALWAYS ACTIVE for all server-side action handlers that coordinate cart operations across multiple requests and require persistent session state between requests.

### Rules

- **R-CART-001** MUST NOT: Cart operations MUST NOT assume synchronous access to session state or block on cookie I/O.

### Verify

```bash
# Discover the project's test suite location and execute server-side action tests
# that validate cart identifier persistence across simulated requests.
find . -type f -name '*.test.*' -o -name '*.spec.*' | grep -i cart | head -5

# Locate the project's linting or static analysis configuration and run checks
# that enforce async function declarations for cart action handlers.
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'tsconfig.json' \) | head -3

# Identify the project's integration test framework and run tests that verify
# cookie storage behavior under error conditions and API failures.
find . -type f -name 'jest.config.*' -o -name 'vitest.config.*' -o -name 'playwright.config.*' | head -3
```

**Accept when:**
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart operations MUST be audited to confirm async cookie API usage with proper error handling and console logging before merge.
</enforcement>