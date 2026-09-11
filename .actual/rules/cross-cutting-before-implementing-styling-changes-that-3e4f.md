# Adopt Utility-First CSS Styling for UI Components: Before Implementing Styling Changes That Rely

These rules are ALWAYS ACTIVE for all UI components and any code interacting with versioned styling libraries or frameworks within the project.

### Rules

- **R-ADOPT-001** MUST: Before implementing styling changes that rely on a versioned styling library or framework, the exact resolved version MUST be verified via the project's lock file.
- **R-ADOPT-002** MUST: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-ADOPT-003** MUST: Before writing code that uses a versioned library, execute in order:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-ADOPT-004** SHOULD: When applying multiple utility classes, especially conditionally, consider using a class concatenation utility to maintain readability and manage complexity.
- **R-ADOPT-005** MUST: Ensure that all custom design elements are first translated into the available utility classes or extended within the styling configuration to maintain consistency.

### Verify

```bash
# Inspect the project's build configuration to confirm the presence and proper setup of the utility-first CSS processing.
# Run the project's development server and visually inspect UI components to ensure styling is applied as expected across different screen sizes and themes.
# Execute the project's UI snapshot or visual regression tests to detect any unintended styling changes.
```

**Accept when:**
- All UI components render with the expected visual appearance and layout according to the design specifications.
- Responsive behaviors and theme variations (e.g., dark mode) function correctly across the application.
- No custom, unmanaged CSS files are introduced for general component styling.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>