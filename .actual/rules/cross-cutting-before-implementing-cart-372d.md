# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Before Implementing Cart

These rules are ALWAYS ACTIVE for all server-side cart operation implementations, cart identifier persistence layers, cart state synchronization operations, and commerce platform integration points.

### Rules

- **R-CART-001** MUST: Before implementing cart operations that depend on framework-specific cookie APIs, discover the exact resolved versions of all server runtime dependencies from the project's lock or resolution artifact.
- **R-CART-002** MUST: Execute lock-version grounding before writing code that uses a versioned library:
  1. Find the dependency manifest in the repo (declares ranges, not installed versions).
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version (authoritative source).
  4. Look up official documentation, changelog, or public API reference for that exact version (do not use training-data recall).
  5. Confirm every API, class, or function exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3–5 per dependency at point of use.
- **R-CART-003** MUST: Implement cart creation operations that atomically execute commerce platform cart instantiation and cookie persistence within a single transaction boundary to prevent identifier inconsistency.
- **R-CART-004** MUST: Implement cart identifier validation on all cart mutation operations to detect and handle missing or invalid cookie values before executing commerce platform API calls.
- **R-CART-005** SHOULD: Implement cart line item lookup optimization using Map-based indexing if cart size metrics indicate performance degradation with linear traversal.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering cart action handlers to verify cookie persistence behavior
# (command varies by build tool — consult project's lock artifact)

# Discover the project's static analysis tooling and execute type checking
# to verify cart operation signatures match the public API contracts
# (command varies by static analysis tool — consult project configuration)

# Discover the project's integration test infrastructure and execute
# commerce platform integration tests to verify cart identifier persistence
# across operation sequences
# (command varies by test framework — consult project configuration)
```

**Accept when:**
- All cart action handler tests pass, demonstrating successful cart identifier persistence and retrieval from cookie storage.
- Type checking confirms cart operation signatures expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie contracts.
- Integration tests verify cart operations maintain identifier consistency across multiple mutation sequences.
- Exact resolved versions of all server runtime dependencies are documented and verified against the lock artifact.
- All cookie API calls are confirmed to exist in the exact version's official documentation before use.

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-CART rules are mandatory before cart operation code is written. Lock-version grounding (R-CART-002) is a prerequisite for any versioned library usage. Test suite execution and type checking must pass before cart operations are merged.
</enforcement>