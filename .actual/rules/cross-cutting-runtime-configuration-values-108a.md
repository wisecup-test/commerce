# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-RUNTIME-001** MUST: Runtime configuration values MUST be validated before use, with invalid values logged and rejected.
- **R-RUNTIME-002** MUST: All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- **R-RUNTIME-003** MUST: Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- **R-RUNTIME-004** MUST: Hardcoded credentials or authentication tokens are prohibited; all credentials MUST be sourced from runtime configuration and validated at startup or first use.
- **R-RUNTIME-005** SHOULD: Centralize request construction logic in a single base function that all domain operations call, ensuring changes to headers or serialization only require updates in one location.
- **R-RUNTIME-006** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.
- **R-RUNTIME-007** SHOULD: Integrate structured logging with appropriate severity levels for authentication failures and ensure logs are aggregated to a centralized monitoring system with alerting.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch\|http" --include="*.ts" --include="*.js" | grep -E "headers|authorization|token" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process\.env\|ENV" --include="*.ts" --include="*.js" | grep -E "domain|token|secret" | head -20

# Review the public API contract exports to ensure domain operations are exposed as named functions
grep -r "export.*function\|export const" --include="*.ts" --include="*.js" | grep -E "cart|create|add|remove|update" | head -20

# Locate the project's test suite directory and identify integration tests
find . -type d -name "__tests__" -o -name "test" -o -name "tests" -o -name "spec" | head -10

# Discover the project's static analysis or linting configuration
find . -maxdepth 2 -name ".eslintrc*" -o -name "eslint.config.*" -o -name "tsconfig.json" | head -10

# Identify the project's environment variable validation logic
grep -r "validate\|check.*env\|required" --include="*.ts" --include="*.js" | grep -E "process\.env|ENV" | head -20
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- No hardcoded credentials or authentication tokens are present in the codebase.
- A centralized base HTTP client function exists that all domain operations delegate to.
- Startup validation checks for required environment variables and fails fast with clear error messages.
- Integration tests mock external API responses and verify request construction, header injection, and error handling.

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklist MUST verify that new external API integrations follow the established client boundary pattern with explicit headers and runtime configuration. Integration tests MUST validate client boundary patterns. Static analysis rules MUST flag hardcoded credentials or inline fetch calls that bypass the established client abstraction layer. Pull requests introducing external API calls without following the established pattern are blocked until refactored. Hardcoded credentials trigger immediate security review and remediation before merge. Integration test failures for client boundary validation block deployment to staging and production environments.
</enforcement>