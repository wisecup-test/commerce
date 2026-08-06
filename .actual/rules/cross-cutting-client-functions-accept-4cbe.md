# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Client Functions Accept

These rules are ALWAYS ACTIVE for all HTTP client functions that integrate with external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, and public API contracts that expose cart or resource operations to internal consumers.

### Rules

- **R-CLIENT-001** MAY: Client functions MAY accept additional headers as parameters to allow caller-specific customization while preserving required authentication headers.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch" --include="*.ts" --include="*.js" | grep -E "(headers|Authorization)" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process.env" --include="*.ts" --include="*.js" | grep -E "(API|TOKEN|SECRET|DOMAIN)" | head -20

# Review the public API contract exports to ensure domain operations are exposed as named functions
grep -r "export" --include="*.ts" --include="*.js" | grep -E "(Cart|cart|fetch)" | head -20

# Locate the project's test suite directory and identify integration tests
find . -type d -name "__tests__" -o -name "test" -o -name "tests" -o -name "spec" | head -10

# Identify the project's environment variable validation logic
grep -r "process.env" --include="*.ts" --include="*.js" | grep -E "(throw|Error|validate|check)" | head -20
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- Client functions that accept additional headers preserve required authentication headers and do not allow them to be overridden by caller-supplied values.
- Base HTTP client function exists and all domain operation functions delegate to it rather than duplicating request construction logic.

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklist MUST verify that new external API integrations follow the established client boundary pattern with explicit headers and runtime configuration. Integration tests MUST mock external API responses and verify request construction, header injection, and error handling. Static analysis rules MUST flag hardcoded credentials or inline fetch calls that bypass the established client abstraction layer.
</enforcement>