# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Public Contracts Expose

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-EX-001** MUST: All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- **R-EX-002** MUST: Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- **R-EX-003** MUST: Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- **R-EX-004** SHOULD: Centralize request construction logic in a single base function that all domain operation functions delegate to, ensuring changes to headers or serialization only require updates in one location.
- **R-EX-005** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.
- **R-EX-006** SHOULD: Integrate structured logging with appropriate severity levels for authentication failures and ensure logs are aggregated to a centralized monitoring system with alerting.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch\|http" --include="*.ts" --include="*.js" | grep -E "headers|Authorization" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process.env\|ENV" --include="*.ts" --include="*.js" | grep -E "token|secret|domain" | head -20

# Review the public API contract exports to ensure domain operations are exposed as named functions
grep -r "export.*function\|export const" --include="*.ts" --include="*.js" | grep -E "cart|create|add|remove|update" | head -20

# Locate the project's test suite directory and identify integration tests
find . -type d -name "__tests__" -o -name "test" -o -name "tests" -o -name "spec" | head -10

# Identify integration tests that verify external API client calls with mocked responses
grep -r "mock\|jest.mock\|sinon" --include="*.test.ts" --include="*.test.js" --include="*.spec.ts" --include="*.spec.js" | head -20

# Discover the project's static analysis or linting configuration
find . -maxdepth 2 -name ".eslintrc*" -o -name "eslint.config.*" -o -name "tsconfig.json" | head -10

# Identify the project's environment variable validation logic
grep -r "validateEnv\|checkEnv\|requiredEnv" --include="*.ts" --include="*.js" | head -20

# Flag hardcoded credentials or inline fetch calls that bypass the established client abstraction layer
grep -r "Authorization.*=.*['\"]" --include="*.ts" --include="*.js" | grep -v "process.env\|config\|env" | head -20
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- Integration tests mock external API responses and verify request construction, header injection, and error handling.
- No hardcoded credentials or authentication tokens are present in the codebase.
- Startup validation checks for required environment variables and fails fast with clear error messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting changes. All rules marked SHOULD are strongly recommended and violations require documented justification.
</enforcement>