# Core Library and Component Integration: Before Implementing Any Code That Uses

These rules are ALWAYS ACTIVE for all new and existing UI components, application pages, and utility and integration modules.

### Rules

- **R-CLI-001** MUST: Before implementing any code that uses a versioned library, the exact resolved version MUST be identified from the project's lock file and its official documentation consulted.

### Verify

```bash
# Discover the project's build command (e.g., `npm run build`, `yarn build`, `gradle build`, `mvn package`)
# Execute the build command to ensure all dependencies resolve correctly.
# Example: <BUILD_COMMAND>

# Discover the project's test command (e.g., `npm test`, `yarn test`, `gradle test`, `mvn test`)
# Execute the test command to validate component and library integrations.
# Example: <TEST_COMMAND>

# Discover the project's dependency manifest (e.g., `package.json`, `pom.xml`, `build.gradle`, `requirements.txt`)
# Inspect the dependency manifest for declared core libraries and components.
# Example: cat <DEPENDENCY_MANIFEST_FILE> | grep "core-library-name"
```

**Accept when:**
- The build process completes without dependency resolution errors.
- All integration tests for components and pages pass.
- The dependency manifest clearly lists the core libraries and components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>