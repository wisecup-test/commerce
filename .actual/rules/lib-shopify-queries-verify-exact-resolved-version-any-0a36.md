# Modularization of Query Fragments within `lib/shopify`: Verify Exact Resolved Version Any Versioned

These rules are ALWAYS ACTIVE for all files within the `lib/shopify` module that define data queries or structures, and for new query definitions or modifications to existing ones that require reusable components.

### Rules

- **R-ADR-001** MUST: Verify the exact resolved version of any versioned dependency by inspecting the project's lock file before implementation.
- **R-ADR-002** MUST: (DISCOVERY POLICY) This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-ADR-003** MUST: (LOCK-VERSION GROUNDING) Before writing code that uses a versioned library, execute in order:
    1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
    2. Identify the build tool from the manifest.
    3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
    4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
    5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
    6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-ADR-004** SHOULD: Place new fragments in a `fragments` directory relative to their consuming queries.
- **R-ADR-005** SHOULD: Ensure fragment names clearly indicate their purpose (e.g., `productFragment.ts`, `seoFragment.ts`).

### Verify

```bash
# Discover the project's linting configuration and execute the linter across the `lib/shopify` module.
# Discover the project's test suite and execute all tests related to `lib/shopify` queries.
# Discover the project's build command and execute a full build of the application.
```

**Accept when:**
- The linter reports no errors or warnings related to fragment imports or definitions.
- All tests within the `lib/shopify` module pass successfully.
- The application builds without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>