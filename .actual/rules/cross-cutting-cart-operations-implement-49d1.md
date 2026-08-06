# Adopt Server Action Pattern for Stateful Cart Operations: Cart Operations Implement

These rules are ALWAYS ACTIVE for all cart state mutation operations, session management operations, and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** SHOULD: Cart operations SHOULD implement optimistic state updates where feasible while ensuring eventual consistency with the external service.
- **R-CART-002** MUST: All cart mutation functions MUST be exported as named async functions with clear parameter contracts and return types to maintain API consistency.
- **R-CART-003** MUST: Cart operations MUST read cart identifiers from cookies on operation entry and write updated identifiers after successful mutations using the framework's cookie API.
- **R-CART-004** MUST: Error handling MUST log failures to the console with sufficient context for debugging while preventing sensitive information leakage in production environments.
- **R-CART-005** MUST: Cart mutation operations MUST implement idempotent operations and compensating transactions to ensure eventual consistency even after partial failures.
- **R-CART-006** MUST: Cookie-based cart identifiers MUST be validated on the server side and protected with secure and httpOnly cookie flags.

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
find . -path "*/integration/*" -o -path "*/e2e/*" | grep -i cart | head -5
```

**Accept when:**
- All cart mutation operations are implemented as exported async functions that coordinate external service calls with session state management.
- Cart operations successfully read and write cart identifiers using cookie-based session management.
- Error handling logs failures to the console and implements appropriate recovery or propagation for all cart operations.
- Cookie-based cart identifiers are validated and protected with secure flags.
- Cart operations implement idempotent behavior and compensating transactions for failure scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart mutation operations MUST comply with these rules before merge approval. Violations detected in code review MUST be rejected. New cart operations MUST demonstrate compliance through test coverage.
</enforcement>