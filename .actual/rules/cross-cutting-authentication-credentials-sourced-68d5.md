# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Authentication Credentials Sourced

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-AUTH-001** MUST: Authentication credentials MUST be sourced from runtime configuration and injected into request headers, never hardcoded in client logic.
- **R-AUTH-002** MUST: All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- **R-AUTH-003** MUST: Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- **R-AUTH-004** MUST: Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- **R-AUTH-005** SHOULD: Centralize request construction logic in a single base function that all domain operations call, ensuring changes to headers or serialization only require updates in one location.
- **R-AUTH-006** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch\|http" --include="*.ts" --include="*.js" | grep -E "headers|Authorization" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process\.env\|import.*config" --include="*.ts" --include="*.js" | head -20

# Review the public API contract exports to ensure domain operations are exposed as named functions
grep -r "export.*function\|export const" --include="*.ts" --include="*.js" | grep -E "cart|Cart|create|add|remove" | head -20

# Examine framework async accessor patterns for cookies and headers
grep -r "cookies\|headers" --include="*.ts" --include="*.js" | grep -E "async|await" | head -20

# Locate integration tests that verify external API client calls with mocked responses
find . -path "*/test*" -o -path "*/__tests__/*" | grep -E "\.test\.|spec\." | head -20

# Verify no hardcoded credentials exist in the codebase
grep -r "Authorization.*Bearer\|token.*=.*['\"]" --include="*.ts" --include="*.js" | grep -v "process\.env\|config\|test" || echo "No hardcoded credentials found"
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- No hardcoded credentials or authentication tokens are present in the codebase.
- Integration tests mock external API responses and verify request construction, header injection, and error handling.
- Startup validation checks for required environment variables and fails fast with clear error messages.

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules R-AUTH-001 through R-AUTH-006 are mandatory for external API client implementations. Violations require immediate remediation before merge.
</enforcement>