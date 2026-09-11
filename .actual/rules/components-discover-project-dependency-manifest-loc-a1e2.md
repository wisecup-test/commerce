# Adopt `next/image` for Image Optimization: Discover Project Dependency Manifest Lock File

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXTIMG-001** MUST: Discover the project's dependency manifest and lock file to determine the exact resolved version of `next/image` before implementation.
- **R-NXTIMG-002** MUST: Find the dependency manifest in the repo.
- **R-NXTIMG-003** MUST: Identify the build tool from the manifest.
- **R-NXTIMG-004** MUST: Inspect the repository lock or resolution artifact to determine the exact resolved version.
- **R-NXTIMG-005** MUST: Look up the official documentation, changelog, or public API reference for that exact version.
- **R-NXTIMG-006** MUST: Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
- **R-NXTIMG-007** MUST: For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-NXTIMG-008** MUST: Ensure all images have meaningful `alt` text for accessibility.
- **R-NXTIMG-009** SHOULD: Consider using a `loader` prop for images hosted on external CDNs.

### Verify

```bash
# Inspect the project's dependency manifest to confirm `next` is listed.
# Run the project's build command and observe image optimization logs.
# Execute the project's test suite to ensure no image-related regressions.
```

**Accept when:**
- `next/image` is the primary component used for displaying images in UI components.
- Images are optimized and served efficiently in production builds.
- No significant performance regressions are introduced by image handling.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>