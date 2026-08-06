# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Modules That Construct

These rules are ALWAYS ACTIVE for all public API modules that construct external client requests with authentication headers, components that render metadata derived from environment configuration, request handlers that validate webhook signatures or revalidation tokens, and utility functions that construct base URLs or service endpoints from environment state.

### Rules

- **R-SECRETS-001** MUST: API modules that construct external client requests with authentication headers shall receive secret values as function parameters or injected dependencies, not by direct environment variable access.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect direct process environment access patterns in modules under the public API scope

# Locate the project's test suite configuration and run integration tests that verify API modules function correctly with injected configuration objects in isolated test environments

# Identify the project's type checking tooling and verify that configuration interfaces enforce required secret properties at compile time
```

**Accept when:**
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules must be executed in the continuous integration pipeline. Code review must enforce configuration abstraction for new API modules. Integration test coverage must verify API modules with injected configuration. Violations block merge requests and trigger automated alerts to the architecture team.
</enforcement>