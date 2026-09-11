# Adoption of clsx for Conditional Class Name Management: Components Use Clsx Conditionally Joining Class

These rules are ALWAYS ACTIVE for UI components and modules that manage conditional class names.

### Rules

- **R-CLS-001** MUST: UI components MUST use `clsx` for conditionally joining class names.

### Verify

```bash
# Inspect the project's dependency manifest for class name utility libraries.
# Examine UI component files for conditional class name assignments.
# Run the project's test suite to ensure styling behaves as expected.
```

**Accept when:**
- The dependency manifest includes a class name utility library.
- UI components consistently use the adopted utility for conditional class names.
- All tests related to component styling pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>