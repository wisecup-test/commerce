# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Cart Operation Error

These rules are ALWAYS ACTIVE for all server-side cart action handlers, cart identifier persistence layers, cart state synchronization operations, and commerce platform integration points.

### Rules

- **R-CART-001** SHOULD: Cart operation error conditions SHOULD log diagnostic information to the console error stream.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering cart action handlers
# to verify cookie persistence behavior and error logging
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Run cart action handler tests
# (Exact command depends on discovered build tool)

# Discover the project's static analysis tooling and execute type checking
# to verify cart operation signatures match the public API contracts

# Discover the project's integration test infrastructure and execute commerce platform
# integration tests to verify cart identifier persistence across operation sequences
```

**Accept when:**
- All cart action handler tests pass, demonstrating successful cart identifier persistence and retrieval from cookie storage
- Type checking confirms cart operation signatures expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie contracts
- Integration tests verify cart operations maintain identifier consistency across multiple mutation sequences
- Console error logging is present in cart operation error paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Test failures block merge for cart operations that bypass cookie-based identifier persistence or omit error logging.
</enforcement>