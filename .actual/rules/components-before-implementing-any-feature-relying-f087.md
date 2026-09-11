# Adoption of clsx for Conditional Class Name Management: Before Implementing Any Feature Relying External

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLS-001** MUST: Before implementing any feature relying on an external dependency, developers MUST verify the exact resolved version of that dependency using the project's lock file.
- **R-CLS-002** MUST: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-CLS-003** MUST: Before writing code that uses a versioned library, execute in order:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-CLS-004** SHOULD: Developers should prioritize using `clsx` over manual string concatenation for any conditional class logic to maintain consistency and readability.

### Verify

```bash
# The ADR provides descriptive verification steps, not direct bash commands.
# Implement these steps using appropriate project-specific tools and commands.
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