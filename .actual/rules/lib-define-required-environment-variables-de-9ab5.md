# Runtime Configuration Management via `process.env`: Define Required Environment Variables Deployment Configurations

These rules are ALWAYS ACTIVE for all application modules that require dynamic configuration values at runtime, especially sensitive credentials and environment-specific settings, or need to adapt behavior based on the deployment environment.

### Rules

- **R-ENV-001** SHOULD: Define all required environment variables in deployment configurations.

### Verify

```bash
# Discover the project's build scripts and execute them to ensure successful compilation.
# Discover the project's test scripts and execute them to ensure all tests pass.
# Discover the project's deployment scripts and simulate a deployment to verify environment variable injection.
```

**Accept when:**
- Application builds successfully without errors related to configuration.
- All automated tests pass, including those that rely on configured values.
- The application starts and operates correctly in a deployed environment, accessing configured values as expected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>