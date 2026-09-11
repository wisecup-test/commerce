# Client Component Directive ("use client") in React: Verify Exact Version React Any Related

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Verify the exact version of React and any related rendering framework from the project's lock file before implementing or modifying client components.
- **R-REACT-002** MUST: The consumer derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-REACT-003** MUST: Before writing code that uses a versioned library, find the dependency manifest in the repo.
- **R-REACT-004** MUST: Before writing code that uses a versioned library, identify the build tool from the manifest.
- **R-REACT-005** MUST: Before writing code that uses a versioned library, inspect the repository lock or resolution artifact to determine the exact resolved version.
- **R-REACT-006** MUST: Before writing code that uses a versioned library, look up the official documentation, changelog, or public API reference for that exact version.
- **R-REACT-007** MUST: Before writing code that uses a versioned library, confirm every API, class, or function you will call exists in that exact version's documentation before using it.
- **R-REACT-008** MUST: For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-REACT-009** MUST: Place the "use client" directive at the very top of the file, before any imports.
- **R-REACT-010** SHOULD: Consider passing interactive elements as children to server components to minimize the client bundle.

### Verify

```bash
# Discover the project's build command and run it to ensure no client/server component boundary errors are reported.
# Discover the project's test command and execute it to confirm client component functionality.
# Discover the project's linter command and run it to check for proper directive placement.
```

**Accept when:**
- The build process completes without errors related to client/server component boundaries.
- All client-side interactive features function as expected in the browser.
- Linter checks pass, ensuring correct "use client" directive usage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>