# Adoption of Internal UI Components and Utility Modules: New Features Modifications Existing Ones Utilize

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- MUST: All new UI features and modifications to existing ones MUST utilize the established internal UI components and utility modules for common patterns and functionalities.

### Verify

```bash
Run the project's frontend build process to ensure all component imports are resolved correctly.
Execute the project's UI tests to confirm component functionality and integration.
Perform a static analysis scan to identify any direct usage of disallowed external UI libraries.
```

**Accept when:**
- The frontend application builds successfully without errors related to component resolution.
- All automated UI tests pass, demonstrating correct component behavior.
- No new direct dependencies on external UI component libraries are introduced without explicit approval.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>