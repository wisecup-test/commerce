# Access Environment Variables Through Process Object for Configuration: Secret Validation Logic

These rules are ALWAYS ACTIVE for all server-side modules accessing external service credentials, API integration layers requiring authentication tokens, utility modules providing configuration access patterns, component rendering logic requiring site metadata, and request handlers validating webhook signatures or revalidation secrets.

### Rules

- **R-SECRET-001** MUST: Secret validation logic SHALL execute before using environment-sourced credentials in external service calls.
- **R-SECRET-002** MUST: Identify all modules that access environment variables and document the expected variable names, types, and whether they are required or optional.
- **R-SECRET-003** MUST: Implement validation logic that executes during application initialization to verify required environment variables are present and contain valid values.
- **R-SECRET-004** MUST: Ensure environment variable access only occurs in server-side execution contexts by reviewing module boundaries and execution environment.
- **R-SECRET-005** MUST: Enforce that no environment variable access occurs in client-side execution contexts.
- **R-SECRET-006** MUST: Verify no sensitive environment variables are embedded in client-accessible bundles.
- **R-SECRET-007** MAY: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

### Verify

```bash
# Discover the project's dependency manifest and identify the testing framework
# Locate and execute the project's test suite to verify environment variable handling

# Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts

# Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles

<enforcement>
Claude Code MUST NOT skip or defer verification. Build failures when static analysis detects environment variable access in client contexts, test failures when required environment variables are not properly validated, and code review rejection for direct environment variable access without validation are mandatory violation handling steps.
</enforcement>