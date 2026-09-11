# Adoption of Internal UI Components and Utility Modules: Developers Introduce New Internal Components Utility

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADOPTION-001** MAY: Developers MAY introduce new internal UI components or utility modules if no existing module adequately addresses the requirement, provided they follow established design and coding standards.

### Verify

```bash
# Run the project's frontend build process to ensure all component imports are resolved correctly.
# Execute the project's UI tests to confirm component functionality and integration.
# Perform a static analysis scan to identify any direct usage of disallowed external UI libraries.
```

**Accept when:**
- The frontend application builds successfully without errors related to component resolution.
- All automated UI tests pass, demonstrating correct component behavior.
- No new direct dependencies on external UI component libraries are introduced without explicit approval.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>