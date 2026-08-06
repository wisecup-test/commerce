# Adopt Server Action Pattern for Stateful Cart Operations: Cart Operations Validate

These rules are ALWAYS ACTIVE for all cart state mutation operations including add, remove, update, and checkout; session management operations that persist cart identifiers; and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** SHOULD: Cart operations SHOULD validate merchandise identifiers against existing cart line items before performing mutations.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite covering cart operation modules to verify async function contracts.
# Discover the project's static analysis or linting configuration and execute it to verify exported function signatures match the expected async patterns.
# Discover the project's integration test suite and execute tests that verify cookie-based session management and external service coordination.
```

**Accept when:**
- All cart mutation operations are implemented as exported async functions that coordinate external service calls with session state management.
- Cart operations successfully read and write cart identifiers using cookie-based session management.
- Error handling logs failures to the console and implements appropriate recovery or propagation for all cart operations.
- Merchandise identifiers are validated against existing cart line items before any mutation is performed.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review verification that all cart mutation functions follow the async export pattern with cookie-based session management is mandatory. Automated testing in continuous integration that validates cart operation contracts and error handling behavior is mandatory. Static analysis checks that verify exported function signatures match the required async patterns are mandatory.
</enforcement>