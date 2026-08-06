# Adopt Server Action Pattern for Stateful Cart Operations: Each Cart Mutation

These rules are ALWAYS ACTIVE for all cart state mutation operations including add, remove, update, and checkout functions, session management operations that persist cart identifiers, and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** MUST: Each cart mutation function MUST handle error conditions by logging errors to the console and implementing appropriate error recovery or propagation.
- **R-CART-002** MUST: All cart mutation functions MUST be exported as named async functions with clear parameter contracts and return types to maintain API consistency.
- **R-CART-003** MUST: Cart mutation functions MUST read cart identifiers from cookies on operation entry and write updated identifiers after successful mutations using the framework's cookie API.
- **R-CART-004** MUST: Error handling MUST log failures with sufficient context for debugging while preventing sensitive information leakage in production environments.

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
- All cart mutation functions demonstrate compliance through test coverage.

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review must confirm all cart mutation functions follow the async export pattern with cookie-based session management. Automated testing in continuous integration must validate cart operation contracts and error handling behavior. Static analysis checks must verify exported function signatures match the required async patterns.
</enforcement>