# Adoption of Core Application Libraries and Next.js Modules: Before Implementing Any Code That Uses

These rules are ALWAYS ACTIVE for all code that interacts with foundational utilities, external services, data caching, or request contexts.

### Rules

- **R-ADOPTION-001** MUST: Before implementing any code that uses a versioned library, the exact resolved version MUST be determined from the project's lock file or resolution artifact. This involves the following steps:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Inspect the project's dependency manifest and lock file to confirm the presence and versions of the core libraries and framework modules.
# Review code changes to ensure new implementations adhere to the specified usage patterns for caching, headers, and external calls.
# Run the project's test suite to validate functionality that relies on these foundational components.
```

**Accept when:**
- The dependency manifest and lock file reflect the expected core libraries and framework modules.
- Code reviews confirm adherence to the established usage patterns for data access, caching, and external communication.
- All relevant tests pass, demonstrating correct integration and functionality.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>