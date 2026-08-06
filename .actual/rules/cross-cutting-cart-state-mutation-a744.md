# Adopt Server Action Pattern for Stateful Cart Operations: Cart State Mutation

These rules are ALWAYS ACTIVE for all cart state mutation operations, session management operations, and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** MUST: All cart state mutation operations MUST be implemented as exported async functions that coordinate external service calls with session state management.
- **R-CART-002** MUST: All cart mutation functions MUST be exported as named async functions with clear parameter contracts and return types to maintain API consistency.
- **R-CART-003** MUST: Cart operations MUST read cart identifiers from cookies on operation entry and write updated identifiers after successful mutations using the framework's cookie API.
- **R-CART-004** MUST: Error handling MUST log failures to the console with sufficient context for debugging while preventing sensitive information leakage in production environments.
- **R-CART-005** SHOULD: Implement optimistic locking or version-based concurrency control at the external service boundary to detect and resolve conflicts from concurrent cart mutations.
- **R-CART-006** SHOULD: Implement cookie signing and validation mechanisms, use secure and httpOnly cookie flags, and validate cart ownership on the server side.
- **R-CART-007** SHOULD: Implement idempotent cart operations and compensating transactions to ensure eventual consistency even after partial failures.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite covering cart operation modules
# to verify async function contracts

# Discover the project's static analysis or linting configuration and execute it to verify exported
# function signatures match the expected async patterns

# Discover the project's integration test suite and execute tests that verify cookie-based session
# management and external service coordination
```

**Accept when:**
- All cart mutation operations are implemented as exported async functions that coordinate external service calls with session state management.
- Cart operations successfully read and write cart identifiers using cookie-based session management.
- Error handling logs failures to the console and implements appropriate recovery or propagation for all cart operations.
- All cart mutation functions are exported as named async functions with clear parameter contracts and return types.
- Cookie operations use the framework's cookie API to manage cart identifiers.
- Test coverage validates cart operation contracts and error handling behavior.
- Static analysis checks verify exported function signatures match the required async patterns.

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review must confirm all cart mutation functions follow the async export pattern with cookie-based session management. Automated testing in continuous integration must validate cart operation contracts and error handling behavior. Static analysis checks must verify exported function signatures match the required async patterns. Pull requests introducing cart operations that do not follow the async function pattern must be rejected during code review.
</enforcement>