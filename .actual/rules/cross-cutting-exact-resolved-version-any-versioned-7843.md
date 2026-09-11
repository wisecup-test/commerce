# Adopt Utility-First CSS Framework for UI Styling: Exact Resolved Version Any Versioned Styling

These rules are ALWAYS ACTIVE for all UI styling dependencies and their usage.

### Rules

- **R-ADOPT-001** MUST: The exact resolved version of any versioned styling dependency MUST be verified via the project's lock file before implementation.
- **R-ADOPT-002** MUST: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ADOPT-003** MUST: Before writing code that uses a versioned library, find the dependency manifest in the repo.
- **R-ADOPT-004** MUST: Before writing code that uses a versioned library, identify the build tool from the manifest.
- **R-ADOPT-005** MUST: Before writing code that uses a versioned library, inspect the repository lock or resolution artifact to determine the exact resolved version.
- **R-ADOPT-006** MUST: Before writing code that uses a versioned library, look up the official documentation, changelog, or public API reference for that exact version, fetching or searching the public internet for version-specific docs.
- **R-ADOPT-007** MUST: Before writing code that uses a versioned library, confirm every API, class, or function to be called exists in that exact version's documentation.
- **R-ADOPT-008** MUST: For version-sensitive behavior, re-run steps 3-5 (R-ADOPT-005 to R-ADOPT-007) per dependency at point of use.

### Verify

```bash
# Inspect UI component files for `className` attributes containing multiple utility classes.
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