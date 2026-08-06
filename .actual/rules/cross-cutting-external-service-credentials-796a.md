# Isolate External API Secrets in Environment Variables with Runtime Validation: External Service Credentials

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-EX-001** MUST: External service credentials SHALL be sourced exclusively from environment variables accessed through the runtime process environment interface.
- **R-EX-002** MUST: Create environment variable validation functions that check for required credentials at module initialization and provide clear error messages indicating which variables are missing and where they should be configured.
- **R-EX-003** MUST: Implement logging filters or sanitization functions that detect and redact secret patterns in error messages and debug output, ensuring validation failures log only the fact of failure without exposing credential values.
- **R-EX-004** MUST: Document required environment variables in deployment configuration templates and provide example values that clearly indicate they are placeholders requiring replacement in production environments.
- **R-EX-005** MUST: Ensure no credential literals appear in source code and all external service authentication uses environment variable sourcing.
- **R-EX-006** MUST: Reject operations with missing credentials and log failures without exposing secret values in error messages or standard output.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.py" --include="*.ts" src/ lib/ | grep -v node_modules | head -20

# Locate and execute tests that validate credential validation logic
find . -name "*test*" -o -name "*spec*" | grep -E "(credential|auth|secret|env)" | head -10

# Inspect deployment configuration templates for environment variable documentation
find . -name "*.env.example" -o -name ".env.sample" -o -name "docker-compose*.yml" -o -name "*.tf" | xargs grep -l "API\|SECRET\|TOKEN\|KEY" 2>/dev/null

# Scan for hardcoded credential patterns in source code
grep -r "password\s*=\|api_key\s*=\|secret\s*=\|token\s*=" --include="*.js" --include="*.py" --include="*.ts" src/ lib/ | grep -v "process\.env\|os\.environ\|getenv" | grep -v test | grep -v example

# Check git history for committed secrets
git log -p --all -S "password=" -S "api_key=" -S "secret=" --oneline | head -20

# Verify error messages do not expose credentials
grep -r "error\|Error\|exception\|Exception" --include="*.js" --include="*.py" --include="*.ts" src/ lib/ | grep -i "credential\|secret\|token\|password" | head -10
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- Static analysis scanning confirms no hardcoded credential patterns exist in the codebase
- Environment variable validation functions are present at module initialization with clear error messages for missing variables
- Logging filters or sanitization functions redact secret patterns in error messages and debug output

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before accepting code changes that involve external service authentication or credential handling.
</enforcement>