# URL Search Parameter Management with Next.js `searchParams`: When Modifying Url Search Parameters Components

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-URL-SEARCH-PARAMS-001** MUST: When modifying URL search parameters, components MUST construct new `URLSearchParams` instances to ensure immutability and proper state updates.

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