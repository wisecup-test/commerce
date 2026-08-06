# Isolate External API Secrets in Environment Variables with Runtime Validation: Client Functions Shall

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-SECRETS-001** MUST: API client functions SHALL validate the presence of required credentials before constructing authenticated HTTP requests and SHALL reject operations with descriptive error messages when credentials are missing.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns in internal API modules
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1
grep -r 'process.env\|os.environ\|ENV\[' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' src/ lib/ | grep -i 'api\|client\|auth' || echo "No environment variable patterns found"

# Locate the project's test suite and execute tests that validate credential validation logic
find . -path '*/test*' -name '*test*' -o -path '*/spec*' -name '*spec*' | head -5

# Inspect the project's deployment configuration templates
find . -name '.env.example' -o -name '.env.template' -o -name 'docker-compose.yml' -o -name 'k8s*.yaml' -o -name 'terraform*.tf' | head -5

# Verify no credential literals in source code
grep -r 'api_key\|secret\|password' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' src/ lib/ | grep -v 'process.env\|os.environ\|ENV\[' | grep -v '.example\|.template' || echo "No hardcoded credentials detected"

# Check for validation logic in API client modules
grep -r 'throw\|raise\|Error\|panic' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' src/ lib/ | grep -i 'credential\|secret\|missing\|required' || echo "No credential validation errors found"
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values
- Environment variable validation functions check for required credentials at module initialization and provide clear error messages indicating which variables are missing
- Logging filters or sanitization functions detect and redact secret patterns in error messages and debug output
- Deployment configuration templates document required environment variables with example values clearly marked as placeholders

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API modules MUST be inspected for environment variable sourcing patterns, credential validation logic, and absence of hardcoded secrets before accepting changes.
</enforcement>