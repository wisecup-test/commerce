# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Cart Line Item

These rules are ALWAYS ACTIVE for all server-side cart action handlers, cart identifier persistence layers, cart state synchronization operations, and commerce platform integration points.

### Rules

- **R-CART-001** MUST: Cart line item queries MUST use merchandise identifier-based lookup to locate existing items within the cart collection.
- **R-CART-002** MUST: Cart creation operations must atomically execute commerce platform cart instantiation and cookie persistence within a single transaction boundary to prevent identifier inconsistency.
- **R-CART-003** MUST: Implement cart identifier validation on all cart mutation operations to detect and handle missing or invalid cookie values before executing commerce platform API calls.
- **R-CART-004** SHOULD: Consider implementing cart line item lookup optimization using Map-based indexing if cart size metrics indicate performance degradation with linear traversal.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering cart action handlers
# to verify cookie persistence behavior

# Discover the project's static analysis tooling and execute type checking to verify cart operation
# signatures match the public API contracts

# Discover the project's integration test infrastructure and execute commerce platform integration tests
# to verify cart identifier persistence across operation sequences
```

**Accept when:**
- All cart action handler tests pass, demonstrating successful cart identifier persistence and retrieval from cookie storage
- Type checking confirms cart operation signatures expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie contracts
- Integration tests verify cart operations maintain identifier consistency across multiple mutation sequences
- Cart line item queries consistently use merchandise identifier-based lookup patterns
- Cart creation operations atomically persist identifiers without orphaning carts

<enforcement>
Claude Code MUST NOT skip or defer verification. Test failures, type checking violations, or integration test failures block acceptance. Code review must verify cookie-based identifier persistence patterns and merchandise identifier lookup implementations before merge.
</enforcement>