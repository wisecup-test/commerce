# React `use` Hook for Context-Provided Promises: Discover Ecosystem Lock File Resolution Artifact

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOCK-001** MUST: Discover the ecosystem's lock file or resolution artifact to determine the exact resolved version of the framework before implementing solutions that rely on its specific API behavior.
- **R-DISC-001** MUST: Derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LOCK-002** MUST: Before writing code that uses a versioned library, execute the following steps in order:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Inspect component files for usage of the `use` hook with Context-provided Promises.
# Run the project's test suite to ensure components consuming Promises from Context behave as expected.
# Review the project's build output for any warnings or errors related to Promise consumption.
```

**Accept when:**
- No direct usage of `useEffect` with `useState` for Promise resolution when `use` hook is applicable.
- Components correctly suspend and resolve when consuming Promises from Context.
- The application's UI remains responsive during asynchronous data loading.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>