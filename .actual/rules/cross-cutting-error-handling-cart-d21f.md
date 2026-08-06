# Use Server-Side Cookie Storage for Cart Session State: Error Handling Cart

These rules are ALWAYS ACTIVE for all server-side action handlers for cart operations including add, remove, update, and checkout; cart creation workflows that establish new session state; cart retrieval operations that depend on persisted identifiers; and webhook handlers that validate requests using header inspection.

### Rules

- **R-CART-ERR-001** SHOULD: Error handling in cart operations SHOULD log failures to the console error stream before propagating exceptions.

### Verify

```bash
# Discover the project's test suite location and execute server-side action tests
# that validate cart identifier persistence across simulated requests.
find . -type f -name '*.test.*' -o -name '*.spec.*' | grep -i cart | head -5

# Locate the project's linting or static analysis configuration and run checks
# that enforce async function declarations for cart action handlers.
find . -maxdepth 2 -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.prettierrc*' \)

# Identify the project's integration test framework and run tests that verify
# cookie storage behavior under error conditions and API failures.
grep -r "describe\|test\|it(" --include="*.test.*" --include="*.spec.*" | grep -i "cookie\|cart" | head -10
```

**Accept when:**
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart operations MUST include try-catch blocks with console error logging for cookie write operations. Violations block CI pipeline merges and require code review revision.
</enforcement>