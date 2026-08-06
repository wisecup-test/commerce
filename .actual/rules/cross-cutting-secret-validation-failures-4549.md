# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Secret Validation Failures

These rules are ALWAYS ACTIVE for all public API modules, components that render metadata derived from environment configuration, request handlers that validate webhook signatures or revalidation tokens, and utility functions that construct base URLs or service endpoints from environment state.

### Rules

- **R-SECRET-001** SHOULD: Secret validation failures should produce structured error objects that identify the missing or invalid configuration key without exposing the secret value.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect direct process environment access patterns in modules under the public API scope
# (Exact command depends on project's linting tool — consult project's build configuration)

# Locate the project's test suite configuration and run integration tests that verify API modules function correctly with injected configuration objects in isolated test environments
# (Exact command depends on project's test runner — consult project's test configuration)

# Identify the project's type checking tooling and verify that configuration interfaces enforce required secret properties at compile time
# (Exact command depends on project's type checker — consult project's type checking configuration)
```

**Accept when:**
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties
- Structured error objects for configuration validation failures identify missing keys without exposing secret values

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification categories (static analysis, integration tests, type checking) MUST pass before accepting changes to public API modules that handle secrets or environment configuration.
</enforcement>