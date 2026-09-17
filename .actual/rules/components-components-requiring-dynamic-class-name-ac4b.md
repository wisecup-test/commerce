# Adopt `clsx` for Conditional Class Name Generation: Components Requiring Dynamic Class Name Generation

These rules are ALWAYS ACTIVE for UI components that require conditional styling based on dynamic conditions.

### Rules

- **R-CLS-001** MUST: Components requiring dynamic class name generation MUST use the `clsx` utility for combining class names.

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