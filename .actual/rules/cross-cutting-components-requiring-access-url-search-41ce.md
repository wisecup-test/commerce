# URL Search Parameter Management with Next.js `searchParams`: Components Requiring Access Url Search Parameters

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-URL-001** MUST: All components requiring access to URL search parameters MUST utilize the `searchParams` object provided by Next.js navigation hooks or `req.nextUrl.searchParams` in server-side contexts.

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