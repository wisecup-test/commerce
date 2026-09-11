# Adopt Utility-First CSS Framework for UI Styling: Developers Prioritize Existing Utility Classes Common

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- R-UI-001 SHOULD: Developers SHOULD prioritize existing utility classes for common styling concerns.
- R-UI-002 MUST: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- R-UI-003 MUST: Before writing code that uses a versioned library, execute the LOCK-VERSION GROUNDING process: 1. Find the dependency manifest. 2. Identify the build tool. 3. Inspect the repository lock or resolution artifact for the exact resolved version. 4. Look up official documentation for that exact version. 5. Confirm every API, class, or function exists in that exact version's documentation. 6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- R-UI-004 SHOULD: Developers SHOULD consult the framework's documentation for available utility classes and best practices for composition.
- R-UI-005 MAY: Consider using tooling to sort and optimize `className` strings for readability and consistency.

### Verify

```bash
# Inspect UI component files for className attributes containing multiple utility classes.
# Run the project's build process and observe the generated CSS output for utility class definitions.
# Execute the project's UI tests to ensure visual consistency.
```

**Accept when:**
- New UI components are styled exclusively using utility classes.
- Existing UI components adhere to the utility-first styling pattern.
- The project's styling output is consistent with a utility-first framework.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>