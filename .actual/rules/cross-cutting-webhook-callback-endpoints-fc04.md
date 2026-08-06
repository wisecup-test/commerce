# Isolate External API Secrets in Environment Variables with Runtime Validation: Webhook Callback Endpoints

These rules are ALWAYS ACTIVE for all internal API modules that authenticate with external HTTP services, webhook handlers that validate incoming requests using shared secrets, client libraries that construct authenticated requests to third-party APIs, and configuration modules that provide credentials to API clients.

### Rules

- **R-WEBHOOK-001** MUST: Webhook or callback endpoints that validate request authenticity using shared secrets SHALL retrieve the validation secret from environment variables and SHALL log validation failures without exposing the secret value.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable access patterns in internal API modules
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -i 'secret\|token\|credential\|auth' | head -20

# Locate the project's test suite and execute tests that validate credential validation logic and error handling for missing environment variables
find . -path '*/test*' -o -path '*/spec*' | grep -E '\.(js|py|go|rb)$' | head -10

# Inspect the project's deployment configuration templates to confirm required environment variables are documented and no credential values are committed to version control
find . -name '.env*' -o -name 'docker-compose*' -o -name 'k8s*' -o -name 'terraform*' -o -name 'helm*' | head -10
grep -r 'secret\|password\|token\|api.?key' --include='.env*' --include='*.yaml' --include='*.yml' --include='*.tf' | grep -v 'PLACEHOLDER\|EXAMPLE\|REPLACE' | head -20
```

**Accept when:**
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification steps MUST be executed before accepting changes to webhook callback endpoints or external API authentication logic.
</enforcement>