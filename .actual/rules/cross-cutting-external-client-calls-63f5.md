# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: External Client Calls

These rules are ALWAYS ACTIVE for all HTTP client calls to external storefront or webhook APIs, functions that construct authenticated requests using environment-sourced credentials, public API contracts that expose cart or resource operations to internal consumers, and runtime configuration validation for secrets and tokens.

### Rules

- **R-EX-001** MUST: External API client calls MUST construct HTTP requests with explicit method, headers object, and body serialization.

### Verify

```bash
# Locate the base HTTP client function that constructs fetch calls with headers and body serialization
grep -r "fetch" --include="*.ts" --include="*.js" --include="*.tsx" --include="*.jsx" | grep -E "(headers|Authorization|Content-Type)" | head -20

# Identify the runtime configuration module that sources environment variables
grep -r "process.env\|import.meta.env" --include="*.ts" --include="*.js" --include="*.tsx" --include="*.jsx" | grep -E "(API|TOKEN|SECRET|DOMAIN)" | head -20

# Review public API contract exports for named domain operation functions
grep -r "export.*\(createCart\|addToCart\|removeFromCart\|updateCart\|shopifyFetch\)" --include="*.ts" --include="*.js" --include="*.tsx" --include="*.jsx"

# Locate integration tests that verify external API client calls with mocked responses
find . -path ./node_modules -prune -o -type f \( -name "*.test.ts" -o -name "*.test.js" -o -name "*.spec.ts" -o -name "*.spec.js" \) -print | xargs grep -l "fetch\|mock.*API" | head -10

# Identify static analysis or linting configuration
find . -maxdepth 2 -type f \( -name ".eslintrc*" -o -name "eslint.config.*" -o -name "biome.json" -o -name ".prettierrc*" \) -print

# Search for hardcoded credentials or inline fetch calls that bypass abstraction
grep -r "Authorization.*Bearer\|token.*=.*['\"]" --include="*.ts" --include="*.js" --include="*.tsx" --include="*.jsx" | grep -v "process.env\|import.meta.env" | head -10
```

**Accept when:**
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.
- Integration tests mock external API responses and verify request construction, header injection, and error handling.
- No hardcoded credentials or authentication tokens are present in the codebase.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external API client calls MUST follow the established pattern with explicit headers and runtime configuration before code is approved.
</enforcement>