# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Cart Mutation Operations

These rules are ALWAYS ACTIVE for all server-side cart mutation operations (addItem, removeItem, updateItemQuantity, redirectToCheckout, createCartAndSetCookie) and their integration points with the commerce platform.

### Rules

- **R-CART-001** MUST: All cart mutation operations (add, remove, update, checkout) MUST retrieve the cart identifier from cookie storage before executing state changes.
- **R-CART-002** MUST: Cart creation operations must atomically execute commerce platform cart instantiation and cookie persistence within a single transaction boundary to prevent identifier inconsistency.
- **R-CART-003** MUST: Implement cart identifier validation on all cart mutation operations to detect and handle missing or invalid cookie values before executing commerce platform API calls.
- **R-CART-004** SHOULD: Implement cart line item lookup using merchandise identifier-based queries (cart.lines.find with merchandise.id comparison) to enable idempotent cart operations and duplicate detection.
- **R-CART-005** SHOULD: Consider implementing cart line item lookup optimization using Map-based indexing if cart size metrics indicate performance degradation with linear traversal.
- **R-CART-006** SHOULD: Augment console logging with structured error tracking and alerting for cart operation error rates to improve production observability.

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
- Cart identifier validation prevents execution of commerce platform API calls with missing or invalid cookie values
- Merchandise identifier-based line item queries are implemented for update and removal operations

<enforcement>
Claude Code MUST NOT skip or defer verification. Test failures block merge for cart operations that bypass cookie-based identifier persistence. Code review rejection applies to cart actions implementing alternative identifier storage mechanisms. Static analysis warnings flag cart operations missing merchandise identifier-based line item queries.
</enforcement>