# Adopt `clsx` for Conditional Class Name Generation: Before Implementing Modifying Code That Uses

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- MUST: Before implementing or modifying code that uses a versioned dependency, consumers MUST discover the project's dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, and consult version-specific documentation to confirm API existence and behavior.

### Verify

```bash
# Inspect relevant UI component files for imports of class name utility functions.
# Review pull requests for new or modified UI components to ensure `clsx` is used for conditional class names.
# Run the project's test suite to ensure no regressions in styling due to class name changes.
```

**Accept when:**
- All UI components requiring conditional class names use the adopted utility.
- No manual string concatenations for conditional class names are found in new code.
- The project's build and test processes complete successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>