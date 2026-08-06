# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Public Cart Action

These rules are ALWAYS ACTIVE for all server-side cart action handlers, cart identifier persistence layers, cart state synchronization operations, and commerce platform integration points.

### Rules

- **R-CART-001** MUST: Public cart action contracts MUST expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie operations.
- **R-CART-002** MUST: Cart creation operations MUST atomically execute commerce platform cart instantiation and cookie persistence within a single transaction boundary to prevent identifier inconsistency.
- **R-CART-003** MUST: Cart identifier validation MUST execute on all cart mutation operations to detect and handle missing or invalid cookie values before executing commerce platform API calls.
- **R-CART-004** SHOULD: Implement cart line item lookup optimization using Map-based indexing if cart size metrics indicate performance degradation with linear traversal.
- **R-CART-005** SHOULD: Augment console logging with structured error tracking and alerting for cart operation error rates to improve production observability.
- **R-CART-006** SHOULD: Monitor cart size distributions and implement indexed lookup structures if cart line counts exceed performance thresholds.

### Verify

```bash
# Discover and run the project's test suite covering cart action handlers
# to verify cookie persistence behavior
find . -name "*.test.*" -o -name "*.spec.*" | head -5

# Discover the project's static analysis tooling and execute type checking
# to verify cart operation signatures match the public API contracts
grep -r "typecheck\|tsc\|eslint" . --include="package.json" --include="tsconfig.json" 2>/dev/null | head -5

# Discover the project's integration test infrastructure and execute
# commerce platform integration tests to verify cart identifier persistence
find . -path "*/integration/*" -o -path "*/e2e/*" | head -5
```

**Accept when:**
- All cart action handler tests pass, demonstrating successful cart identifier persistence and retrieval from cookie storage
- Type checking confirms cart operation signatures expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie contracts
- Integration tests verify cart operations maintain identifier consistency across multiple mutation sequences
- Cart creation retry logic with idempotency guarantees is implemented and monitored
- Structured error tracking is in place for cart operation failures

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart action implementations MUST pass the test suite and type checking before merge. Code review MUST verify cookie-based identifier persistence patterns and reject alternative storage mechanisms. Static analysis warnings for missing merchandise identifier-based line item queries MUST be resolved.
</enforcement>