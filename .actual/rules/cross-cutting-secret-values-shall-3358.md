# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Secret Values Shall

These rules are ALWAYS ACTIVE for all modules that export public API contracts for external service integration, components that render metadata derived from environment configuration, request handlers that validate webhook signatures or revalidation tokens, and utility functions that construct base URLs or service endpoints from environment state.

### Rules

- **R-SECRET-001** MUST_NOT: Secret values shall not be logged to standard output or error streams, even in validation failure scenarios.
- **R-SECRET-002** MUST: Create a configuration module that exports typed interfaces for each category of secret: service credentials, domain identifiers, and validation tokens. Initialize and validate all required secrets at module load time.
- **R-SECRET-003** MUST: Refactor existing API modules to accept configuration objects as constructor parameters or function arguments rather than accessing process environment directly. Prioritize modules that construct external client requests with authentication headers.
- **R-SECRET-004** MUST: Implement structured error types for configuration validation failures that identify the missing key without exposing secret values. Ensure error messages are safe to log and display in development environments.
- **R-SECRET-005** SHOULD: Implement static analysis rules to detect direct process environment access in API modules and enforce through continuous integration checks.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules
# that detect direct process environment access patterns in modules under the public API scope
echo "Executing static analysis for direct process.env access in public API modules..."

# Locate the project's test suite configuration and run integration tests that verify
# API modules function correctly with injected configuration objects in isolated test environments
echo "Running integration tests with mocked configuration objects..."

# Identify the project's type checking tooling and verify that configuration interfaces
# enforce required secret properties at compile time
echo "Verifying type checking for configuration interfaces..."
```

**Accept when:**
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties
- Configuration module exports typed interfaces for service credentials, domain identifiers, and validation tokens
- All API modules accept configuration objects as constructor parameters or function arguments
- Structured error types for configuration validation failures do not expose secret values

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules executed in continuous integration pipeline, code review checklist enforcement, and integration test coverage requirements are mandatory. Violations result in continuous integration pipeline failure and code review blocking.
</enforcement>