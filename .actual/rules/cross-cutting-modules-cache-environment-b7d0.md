# Isolate External API Secrets in Environment Variables with Runtime Validation: Modules Cache Environment

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-SECRETS-001** MUST: Source all external service credentials exclusively from environment variables without hardcoding credential values in source code.
- **R-SECRETS-002** MUST: Implement validation logic that rejects operations with missing credentials and fails fast at module initialization or request time.
- **R-SECRETS-003** MUST: Ensure validation failures log only the fact of failure without exposing secret values in error messages or standard output.
- **R-SECRETS-004** SHOULD: Implement logging filters or sanitization functions that detect and redact secret patterns in error messages and debug output.
- **R-SECRETS-005** SHOULD: Document all required environment variables in deployment configuration templates with example placeholder values.
- **R-SECRETS-006** MAY: Cache environment variable reads at module scope to avoid repeated process environment lookups, provided the deployment model does not require runtime credential rotation.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns
grep -r "process\.env\|os\.environ" --include="*.js" --include="*.ts" --include="*.py" src/ | grep -v node_modules

# Scan for hardcoded credential patterns
grep -r "password\|secret\|token\|api[_-]?key" --include="*.js" --include="*.ts" --include="*.py" src/ | grep -v "process\.env\|os\.environ\|getenv" | head -20

# Locate and execute test suite for credential validation
find . -name "*test*" -o -name "*spec*" | grep -E "\.(js|ts|py)$" | head -5

# Inspect deployment configuration templates for environment variable documentation
find . -name ".env*" -o -name "*deploy*" -o -name "docker-compose*" -o -name "*k8s*" | grep -v node_modules

# Check version control history for committed credential values
git log -p --all -S "password\|secret\|token" -- . | head -50
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- Static analysis scanning confirms no hardcoded credential patterns exist in the codebase
- Deployment pipeline validation checks for required environment variables before promoting builds to production

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklist MUST require verification that no credential literals appear in source code. Static analysis MUST scan for hardcoded credential patterns. Deployment pipeline MUST validate required environment variables before production promotion. Pull requests containing hardcoded credentials MUST be blocked from merge.
</enforcement>