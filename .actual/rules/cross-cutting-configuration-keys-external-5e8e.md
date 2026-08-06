# Source Runtime Configuration from Environment Variables for External Service Integration: Configuration Keys External

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, webhook handlers that validate incoming requests, configuration initialization code, and error handling logic that reports configuration issues.

### Rules

- **R-CONFIG-001** MUST: Configuration keys for external service credentials MUST follow a namespaced naming convention that identifies the service and credential type.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | grep -E '(config|environment|integration)' | head -10

# Identify static analysis or linting rules that detect direct credential usage
grep -r 'hardcoded\|credential\|secret' . --include='*.json' --include='*.yaml' --include='*.yml' --include='*.toml' 2>/dev/null | head -5

# Verify no hardcoded credentials in API modules
grep -r 'password\|token\|secret\|api_key' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.java' 2>/dev/null | grep -v 'process.env\|os.environ\|getenv' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Configuration keys follow a service-prefixed naming pattern (e.g., `EXTERNAL_SERVICE_DOMAIN`, `EXTERNAL_SERVICE_TOKEN`, `EXTERNAL_SERVICE_REVALIDATION_SECRET`)

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing hardcoded credentials or endpoints are blocked until refactored to use environment configuration. Static analysis failures for credential detection trigger build failures and require remediation before merge.
</enforcement>