# Core Library and Component Integration: Core Application Logic Elements Composed Established

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CORE-001** MUST: Core application logic and UI elements MUST be composed using established internal components and libraries.

### Verify

```bash
# Discover the project's build command and execute it to ensure all dependencies resolve correctly.
# Example: npm run build or yarn build or ./gradlew build

# Discover the project's test command and execute it to validate component and library integrations.
# Example: npm test or yarn test or ./gradlew test

# Discover the project's dependency manifest and inspect it for declared core libraries and components.
# Example: cat package.json or cat build.gradle
```

**Accept when:**
- The build process completes without dependency resolution errors.
- All integration tests for components and pages pass.
- The dependency manifest clearly lists the core libraries and components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>