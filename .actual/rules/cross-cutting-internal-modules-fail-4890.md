# Isolate External API Secrets in Environment Variables with Runtime Validation: Internal Modules Fail

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-SECRETS-001** MUST: Internal API modules MUST source external service credentials exclusively from environment variables without hardcoded values in source code.
- **R-SECRETS-002** MUST: Validation logic MUST reject operations with missing credentials and log failures without exposing secret values in error messages or standard output.
- **R-SECRETS-003** SHOULD: Internal API modules SHOULD fail fast during initialization or first use if required credentials are unavailable, rather than deferring errors to runtime request handling.
- **R-SECRETS-004** MUST: Deployment configuration MUST document all required environment variables and version control history MUST contain no committed credential values.
- **R-SECRETS-005** MUST: Logging filters or sanitization functions MUST detect and redact secret patterns in error messages and debug output.
- **R-SECRETS-006** MUST: Environment variable validation functions MUST check for required credentials at module initialization and provide clear error messages indicating which variables are missing and where they should be configured.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -i 'api\|secret\|token\|credential' | head -20

# Locate and execute tests validating credential validation logic
find . -path '*/test*' -o -path '*/spec*' | grep -E '(test|spec)\.(js|py|go|rb)$' | head -5

# Inspect deployment configuration templates
find . -name '*.env.example' -o -name '.env.sample' -o -name 'docker-compose.yml' -o -name 'k8s*.yaml' | head -10

# Scan for hardcoded credential patterns
grep -r 'password\|secret\|token\|api[_-]?key' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -v 'process\.env\|os\.environ\|getenv\|example\|sample\|TODO\|FIXME' | head -20

# Verify no credential values in version control
git log --all --full-history -S 'sk_live_\|pk_live_\|AKIA' --oneline | head -10
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- Environment variable validation functions are present at module initialization with clear error messages for missing variables
- Logging filters or sanitization functions detect and redact secret patterns in error messages and debug output
- No credential literals appear in source code across the repository

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be verified before accepting changes to internal API modules, webhook handlers, client libraries, or configuration modules that handle external service authentication.
</enforcement>