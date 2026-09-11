# URL Search Parameter Management with Next.js `searchParams`: Before Implementing Any Code That Relies

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DEP-001** MUST: Before implementing any code that relies on a versioned dependency, the consumer MUST locate the project's dependency manifest, identify the build tool, inspect the repository's lock or resolution artifact to determine the exact resolved version, and consult the official documentation for that specific version to confirm API existence and behavior.

### Verify

```bash
# Inspect relevant component files for usage of URL search parameter APIs.
# Run the application's test suite to ensure URL state transitions are correct.
# Manually test application features that rely on URL query parameters.
```

**Accept when:**
- All components correctly read URL search parameters.
- URL modifications result in expected application state changes and navigation.
- No direct mutations of the `searchParams` object are observed without proper `URLSearchParams` construction.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>