# Adopt Next.js Framework for Web Application Development: Custom Utility Modules Components Used Encapsulate

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NEXTJS-001** MAY: Custom utility modules and components MAY be used to encapsulate shared logic and UI elements.

### Verify

```bash
# Inspect the project's dependency manifest to identify the declared framework.
# Execute the project's build command to confirm successful compilation.
# Run the project's test suite to validate core functionalities.
```

**Accept when:**
- The dependency manifest explicitly lists the framework.
- The build process completes without errors.
- All automated tests pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>