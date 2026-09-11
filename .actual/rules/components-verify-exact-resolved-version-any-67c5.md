# Adoption of clsx for Conditional Class Name Management: Verify Exact Resolved Version Any External

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLS-001** MUST: Verify the exact resolved version of any external library via the project's lock file before implementation.

### Verify

```bash
# Inspect component files for instances of class name concatenation.
# Run the project's linting and testing suite to identify any styling inconsistencies.
```

**Accept when:**
- All new and modified UI components use the `clsx` utility for dynamic class name generation.
- Code reviews confirm consistent application of the `clsx` utility where appropriate.
- No new instances of manual conditional string concatenation for class names are introduced.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>