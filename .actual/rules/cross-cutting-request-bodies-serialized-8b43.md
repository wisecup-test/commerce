# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Request Bodies Serialized

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-REQ-001** MUST: Request bodies MUST be serialized as JSON with conditional field inclusion based on presence of query and variables parameters.
- **R-REQ-002** MUST: All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- **R-REQ-003** MUST: Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- **R-REQ-004** MUST: Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- **R-REQ-005** MUST: Hardcoded credentials or authentication tokens are prohibited; all credentials must be sourced from environment variables or runtime configuration.
- **R-REQ-006** SHOULD: Centralize request construction logic in a single base function that all domain operations call to ensure changes to headers or serialization only require updates in one location.
- **R-REQ-007** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch\|request" --include="*.ts" --include="*.js" | grep -E "headers|body|JSON\.stringify" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process\.env\|import.*config" --include="*.ts" --include="*.js" | head -20

# Review the public API contract exports to ensure domain operations are exposed as named functions
grep -r "export.*function\|export const" --include="*.ts" --include="*.js" | grep -E "cart|create|add|remove|update" | head -20

# Examine async accessor patterns for cookies and headers
grep -r "cookies\|headers" --include="*.ts" --include="*.js" | grep -E "async|await" | head -20

# Locate the project's test suite directory and identify integration tests
find . -type d -name "__tests__" -o -name "test" -o -name "tests" -o -name "spec" | head -10

# Verify no hardcoded credentials exist
grep -r "Authorization.*Bearer\|token.*=.*['\"]" --include="*.ts" --include="*.js" | grep -v "process\.env" | grep -v "config" | head -20

# Check for inline fetch calls that bypass abstraction
grep -r "fetch(" --include="*.ts" --include="*.js" | grep -v "shopifyFetch\|apiClient\|baseClient" | head -20
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- No hardcoded credentials or authentication tokens are present in the codebase.
- All domain operation functions delegate to a centralized base HTTP client function rather than duplicating request construction logic.
- Integration tests verify external API client calls with mocked responses and validate request construction, header injection, and error handling.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that constructs external API requests. Violations must be identified and remediated before merge.
</enforcement>