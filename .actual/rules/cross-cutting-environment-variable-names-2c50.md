# Isolate External API Secrets in Environment Variables with Runtime Validation: Environment Variable Names

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-ENV-001** SHOULD: Environment variable names for secrets SHOULD follow a consistent naming convention that clearly identifies the external service and credential type.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns in internal API modules
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -i 'api\|secret\|token\|credential' | head -20

# Locate the project's test suite and execute tests that validate credential validation logic and error handling for missing environment variables
find . -path './node_modules' -prune -o -path './venv' -prune -o -type f \( -name '*.test.js' -o -name '*.test.py' -o -name '*_test.go' -o -name '*_spec.rb' \) -print | head -10

# Inspect the project's deployment configuration templates to confirm required environment variables are documented and no credential values are committed to version control
find . -name '.env.example' -o -name '.env.template' -o -name 'docker-compose.yml' -o -name 'k8s*.yaml' -o -name 'terraform*.tf' | xargs grep -l 'ENV\|environment' 2>/dev/null

# Scan for hardcoded credential patterns in source code
grep -r 'password\|secret\|token\|api_key' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -v 'process\.env\|os\.environ\|getenv\|ENV\[' | grep -v '.example\|.template' | head -20
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- Environment variable names follow a consistent naming convention that clearly identifies the external service and credential type

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before approving changes to internal API modules, webhook handlers, client libraries, and configuration modules.
</enforcement>