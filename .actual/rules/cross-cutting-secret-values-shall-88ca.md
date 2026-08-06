# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Secret Values Shall

These rules are ALWAYS ACTIVE for all public API modules that export contracts for external service integration, components that render metadata derived from environment configuration, request handlers that validate webhook signatures or revalidation tokens, and utility functions that construct base URLs or service endpoints from environment state.

### Rules

- **R-SECRET-001** MUST: Secret values shall be validated for presence and format at configuration initialization time, not at point of use within API request handlers or component render paths.
- **R-SECRET-002** MUST: Public API modules shall accept configuration objects as constructor parameters or function arguments rather than accessing process environment variables directly.
- **R-SECRET-003** MUST: Configuration validation failures shall use structured error types that identify the missing key without exposing secret values.
- **R-SECRET-004** MUST: Error messages for configuration failures shall be safe to log and display in development environments.
- **R-SECRET-005** SHOULD: Create a configuration module that exports typed interfaces for each category of secret: service credentials, domain identifiers, and validation tokens.
- **R-SECRET-006** MAY: Implement time-based cache invalidation for environments with frequent secret rotation requirements, provided process restart procedures are documented.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting rules
# that detect direct process environment access patterns in public API modules
find . -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.pylintrc' -o -name 'pyproject.toml' | head -1

# Locate the project's test suite configuration and run integration tests
# that verify API modules function correctly with injected configuration objects
find . -name 'jest.config.*' -o -name 'vitest.config.*' -o -name 'pytest.ini' -o -name 'tox.ini' | head -1

# Identify the project's type checking tooling and verify configuration interfaces
find . -name 'tsconfig.json' -o -name 'mypy.ini' -o -name '.mypy.ini' | head -1
```

**Accept when:**
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties
- All public API modules use dependency injection or constructor parameters to receive configuration objects
- Configuration validation errors are caught at initialization time and do not expose secret values in error messages

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules MUST be executed in the continuous integration pipeline. Code review MUST enforce configuration abstraction for new API modules. Integration test coverage MUST be verified for all API modules with injected configuration. Violations MUST block merge requests and trigger automated alerts to the architecture team.
</enforcement>