# Adopt Server Action Pattern for Stateful Cart Operations: Public Cart Operation

These rules are ALWAYS ACTIVE for all cart state mutation operations, session management operations, and external service integration functions that coordinate cart state with remote datastores.

### Rules

- **R-CART-001** MUST: All public cart operation contracts MUST be exported as named async functions from the module boundary.

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

<enforcement>
Clause Code MUST NOT skip or defer verification. All cart mutation functions must follow the async export pattern with cookie-based session management before merge approval.
</enforcement>