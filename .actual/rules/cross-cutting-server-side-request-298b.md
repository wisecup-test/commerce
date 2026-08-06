# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Server Side Request

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-SSREQ-001** SHOULD: Server-side request context SHOULD be accessed through framework-provided async accessors to maintain separation from business logic.
- **R-SSREQ-002** MUST: All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- **R-SSREQ-003** MUST: Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- **R-SSREQ-004** MUST: Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- **R-SSREQ-005** MUST: Hardcoded credentials or authentication tokens are prohibited; all credentials MUST be sourced from environment variables or runtime configuration.
- **R-SSREQ-006** SHOULD: Centralize request construction logic in a single base function that all domain operations call to ensure changes to headers or serialization only require updates in one location.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch\|http" --include="*.ts" --include="*.js" | grep -E "headers|Authorization" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process\.env\|ENV" --include="*.ts" --include="*.js" | grep -E "token|secret|domain" | head -20

# Review the public API contract exports
grep -r "export.*function\|export.*const" --include="*.ts" --include="*.js" | grep -E "Cart|cart|fetch" | head -20

# Examine async accessor patterns for cookies and headers
grep -r "cookies\|headers" --include="*.ts" --include="*.js" | grep -E "async|await" | head -20

# Locate the project's test suite directory and identify integration tests
find . -type f -name "*.test.ts" -o -name "*.test.js" -o -name "*.spec.ts" -o -name "*.spec.js" | head -20

# Identify static analysis or linting configuration
find . -type f -name ".eslintrc*" -o -name "tsconfig.json" -o -name "biome.json" | head -10

# Identify environment variable validation logic
grep -r "process\.env" --include="*.ts" --include="*.js" | grep -E "if|throw|validate" | head -20
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- No hardcoded credentials or authentication tokens are present in the codebase.
- All domain operation functions delegate to a centralized base HTTP client function rather than duplicating request construction logic.
- Framework-provided async accessors for cookies and headers are consistently used across all functions that require request context.
- Integration tests mock external API responses and verify request construction, header injection, and error handling.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before approving pull requests that introduce or modify external API client calls.
</enforcement>