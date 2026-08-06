# Isolate External API Secrets in Environment Variables with Runtime Validation: Secret Values Shall

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-SECRET-001** MUST: Secret values SHALL NOT be committed to version control, embedded as string literals in source code, or logged to standard output or error streams.
- **R-SECRET-002** MUST: All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code.
- **R-SECRET-003** MUST: Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output.
- **R-SECRET-004** MUST: Create environment variable validation functions that check for required credentials at module initialization and provide clear error messages indicating which variables are missing and where they should be configured.
- **R-SECRET-005** MUST: Implement logging filters or sanitization functions that detect and redact secret patterns in error messages and debug output, ensuring validation failures log only the fact of failure without exposing credential values.
- **R-SECRET-006** SHOULD: Document required environment variables in deployment configuration templates and provide example values that clearly indicate they are placeholders requiring replacement in production environments.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns in internal API modules
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" src/ | grep -v node_modules | head -20

# Locate the project's test suite and execute tests that validate credential validation logic and error handling for missing environment variables
find . -name "*.test.*" -o -name "*.spec.*" | grep -i secret | head -10

# Inspect the project's deployment configuration templates to confirm required environment variables are documented and no credential values are committed to version control
grep -r "API_KEY\|SECRET\|TOKEN\|PASSWORD" --include=".env*" --include="*.yml" --include="*.yaml" --include="*.json" . 2>/dev/null | grep -v node_modules | grep -v ".git"

# Scan for hardcoded credential patterns in source code
grep -r "api_key\s*=\|secret\s*=\|token\s*=\|password\s*=" --include="*.js" --include="*.ts" --include="*.py" src/ | grep -v "process\.env\|os\.environ\|getenv" | head -20
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- No hardcoded credential literals appear in source code scanning results
- Environment variable validation functions exist and are called at module initialization
- Logging filters or sanitization functions are implemented to redact secret patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before accepting code changes that involve external API authentication or credential handling.
</enforcement>