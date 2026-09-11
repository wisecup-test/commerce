# Adoption of React Client Components for Interactive UI: Consumers Verify Exact Version Any Versioned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- R-VER-001 MUST: Consumers MUST verify the exact version of any versioned dependency by inspecting the project's lock file before implementation.

### Verify

```bash
# LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
# 1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
# 2. Identify the build tool from the manifest.
# 3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
# 4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
# 5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
# 6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
```

**Accept when:**
- The exact version of all versioned dependencies used in implementation has been identified from the project's lock file.
- All APIs, classes, or functions used from versioned dependencies are confirmed to exist in the identified exact version's official documentation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>