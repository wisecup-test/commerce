# Adopt Next.js Framework for Web Application Development: Project Utilize Next Framework New Web

These rules are ALWAYS ACTIVE for all new web application development.

### Rules

- **R-NEXTJS-001** MUST: The project MUST utilize the Next.js framework for all new web application development.
- **R-NEXTJS-002** MUST: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-NEXTJS-003** MUST: Before writing code that uses a versioned library, execute in order:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

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