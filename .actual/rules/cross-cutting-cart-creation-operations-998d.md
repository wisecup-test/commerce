# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Cart Creation Operations

These rules are ALWAYS ACTIVE for all server-side cart action handlers, cart identifier persistence layers, cart state synchronization operations, and commerce platform integration points.

### Rules

- **R-CART-001** MUST: Cart creation operations MUST atomically persist the cart identifier to cookie storage immediately after successful cart instantiation.
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

<enforcement>
Claude Code MUST NOT skip or defer verification. Test failures block merge for cart operations that bypass cookie-based identifier persistence. Code review rejection applies to cart actions implementing alternative identifier storage mechanisms. Static analysis warnings flag cart operations missing merchandise identifier-based line item queries. Violations require documented architectural justification and technical lead approval.
</enforcement>