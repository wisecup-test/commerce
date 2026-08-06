# Adopt Server Action Pattern for Stateful Cart Operations: Cart Operations Use

These rules are ALWAYS ACTIVE for all cart state mutation operations, session management operations, and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** MUST: Cart operations MUST use cookie-based session identifiers to maintain cart state continuity across requests.
- **R-CART-002** MUST: All cart mutation operations (add, remove, update, checkout) MUST be implemented as exported async functions with clear parameter contracts and return types.
- **R-CART-003** MUST: Cart operations MUST read cart identifiers from cookies on operation entry and write updated identifiers after successful mutations using the framework's cookie API.
- **R-CART-004** MUST: Error handling MUST log failures to the console with sufficient context for debugging while preventing sensitive information leakage in production environments.
- **R-CART-005** SHOULD: Implement optimistic locking or version-based concurrency control at the external service boundary to detect and resolve conflicts from concurrent cart mutations.
- **R-CART-006** SHOULD: Implement cookie signing and validation mechanisms, use secure and httpOnly cookie flags, and validate cart ownership on the server side.
- **R-CART-007** SHOULD: Implement idempotent cart operations and compensating transactions to ensure eventual consistency after partial failures.

### Verify

```bash
# Discover and run the project's test suite covering cart operation modules
# to verify async function contracts
find . -name "*.test.*" -o -name "*.spec.*" | grep -i cart | head -5

# Discover and execute the project's static analysis or linting configuration
# to verify exported function signatures match async patterns
grep -r "export.*async.*function" --include="*.ts" --include="*.js" | grep -i cart

# Discover and run integration tests verifying cookie-based session management
# and external service coordination
find . -name "*integration*" -o -name "*e2e*" | grep -i cart | head -5
```

**Accept when:**
- All cart mutation operations are implemented as exported async functions that coordinate external service calls with session state management.
- Cart operations successfully read and write cart identifiers using cookie-based session management.
- Error handling logs failures to the console and implements appropriate recovery or propagation for all cart operations.
- Static analysis confirms exported function signatures match the required async patterns.
- Test coverage demonstrates compliance with async function contracts and error handling behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart mutation operations must demonstrate compliance with R-CART-001 through R-CART-004 before code acceptance. Violations detected in pull requests must be rejected during code review.
</enforcement>