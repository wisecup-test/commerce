# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Environment Sourced Secrets

These rules are ALWAYS ACTIVE for all public API modules that export contracts for external service integration, components that render metadata derived from environment configuration, request handlers that validate webhook signatures or revalidation tokens, and utility functions that construct base URLs or service endpoints from environment state.

### Rules

- **R-ENV-001** MUST: All environment-sourced secrets required by public API modules shall be retrieved through a dedicated configuration abstraction layer that isolates direct process environment access from API contract implementations.
- **R-ENV-002** MUST: Create a configuration module that exports typed interfaces for each category of secret: service credentials, domain identifiers, and validation tokens, with initialization and validation of all required secrets at module load time.
- **R-ENV-003** MUST: Refactor existing API modules to accept configuration objects as constructor parameters or function arguments rather than accessing process environment directly, prioritizing modules that construct external client requests with authentication headers.
- **R-ENV-004** MUST: Implement structured error types for configuration validation failures that identify the missing key without exposing secret values, ensuring error messages are safe to log and display in development environments.
- **R-ENV-005** MUST: Implement fail-fast validation at application startup with clear error messages identifying missing or invalid configuration keys.
- **R-ENV-006** SHOULD: Implement static analysis rules to detect direct process environment access in API modules and enforce through continuous integration checks.
- **R-ENV-007** MAY: Implement time-based cache invalidation for environments with frequent secret rotation requirements, or document secret rotation procedures requiring process restart.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules
# that detect direct process environment access patterns in modules under the public API scope
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
- Configuration module exports typed interfaces for service credentials, domain identifiers, and validation tokens
- All API modules accept configuration objects as constructor parameters or function arguments
- Configuration validation errors are structured and do not expose secret values in error messages

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules MUST be executed in the continuous integration pipeline. Code review MUST enforce configuration abstraction for new API modules. Integration test coverage MUST be verified for API modules with injected configuration. Violations MUST cause continuous integration pipeline failure and block merge requests.
</enforcement>