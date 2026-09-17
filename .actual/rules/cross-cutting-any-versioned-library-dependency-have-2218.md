# Client-Side React and Next.js Navigation Core Library Adoption: Any Versioned Library Dependency Have Its

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Any versioned library dependency MUST have its exact resolved version verified via the project's lock file before implementation.

### Verify

```bash
# Discover the project's build command and execute it.
build_command=$(discover_build_command)
$build_command

# Discover the project's test command and execute it.
test_command=$(discover_test_command)
$test_command

# Discover the project's lint command and execute it.
lint_command=$(discover_lint_command)
$lint_command
```

**Accept when:**
- The build process completes without errors.
- All automated tests pass successfully.
- Linting reports no critical issues or warnings related to client-side component usage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>