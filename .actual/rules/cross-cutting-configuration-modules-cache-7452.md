# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Configuration Modules Cache

These rules are ALWAYS ACTIVE for all public API modules, external client boundary components, request handlers that validate webhook signatures, and utility functions that construct service endpoints from environment state.

### Rules

- **R-CONFIG-001** MUST: Create a configuration module that exports typed interfaces for each category of secret: service credentials, domain identifiers, and validation tokens. Initialize and validate all required secrets at module load time.
- **R-CONFIG-002** MUST: Refactor existing API modules to accept configuration objects as constructor parameters or function arguments rather than accessing process environment directly. Prioritize modules that construct external client requests with authentication headers.
- **R-CONFIG-003** MUST: Implement structured error types for configuration validation failures that identify the missing key without exposing secret values. Ensure error messages are safe to log and display in development environments.
- **R-CONFIG-004** MAY: Configuration modules may cache validated secret values for the lifetime of the process to avoid repeated environment lookups.
- **R-CONFIG-005** MUST: Implement static analysis rules to detect direct process environment access in API modules and enforce through continuous integration checks.
- **R-CONFIG-006** SHOULD: Document secret rotation procedures requiring process restart, or implement time-based cache invalidation for environments with frequent rotation requirements.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules
# that detect direct process environment access patterns in modules under the public API scope
grep -r "process\.env" src/api --include="*.js" --include="*.ts" || echo "No direct process.env access found"

# Locate the project's test suite configuration and run integration tests
# that verify API modules function correctly with injected configuration objects
npm test -- --testPathPattern="api.*config" --coverage

# Identify the project's type checking tooling and verify that configuration interfaces
# enforce required secret properties at compile time
npm run type-check
```

**Accept when:**
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties
- Configuration module exports are used consistently across all public API entry points

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and continuous integration pipeline enforcement.
</enforcement>