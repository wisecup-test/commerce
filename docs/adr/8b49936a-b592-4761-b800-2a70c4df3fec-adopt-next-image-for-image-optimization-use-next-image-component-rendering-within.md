# Adopt `next/image` for Image Optimization: Use Next Image Component Rendering Within

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- Modern web applications require optimized image delivery for performance.
- Efficient image loading and rendering are critical for user experience.
- The project utilizes Next.js, which provides a built-in image component.
- UI components frequently display various types of images.

## Problem Statement

Ensuring consistent, performant, and optimized image handling across all UI components without manual optimization efforts.

## Decision

1. MUST: MUST use the `next/image` component for all image rendering within UI components.

## Policy Block

- MUST MUST use the `next/image` component for all image rendering within UI components.

In scope:
- All UI components that display images.

Out of scope:
- Images served from external, unoptimized sources where `next/image` cannot be applied.
- Small decorative images that do not significantly impact performance and are not part of the main content.

## Rationale

- `next/image` provides automatic image optimization, including lazy loading, responsive sizing, and modern formats (e.g., WebP), improving performance out-of-the-box.
- Centralizing image handling through a dedicated component ensures consistency and reduces boilerplate.
- Leveraging a framework-provided solution aligns with the project's Next.js architecture.

## Consequences

Positive:
- Improved page load times
- Better Core Web Vitals scores
- Reduced developer effort for image optimization
- Consistent image behavior across the application

Negative:
- Potential for increased build times due to image processing
- Initial learning curve for `next/image` properties
- Dependency on Next.js for image handling

## Alternatives

- Manual `<img>` tags with custom optimization. (rejected)
  Rejected because: Requires significant manual effort for optimization, inconsistent results, and potential for performance regressions.
  When valid: For very simple, static sites with minimal image content where build-time optimization is not desired.
- Third-party image optimization service/library. (rejected)
  Rejected because: Introduces an additional dependency and potential vendor lock-in, `next/image` provides sufficient capabilities for current needs.
  When valid: For projects not using Next.js or requiring advanced image manipulation features not offered by `next/image`.

## Risks

- Performance degradation if `next/image` is misconfigured (e.g., incorrect `width`/`height`, excessive `priority`).
  Mitigation: Implement linting rules and code reviews to ensure correct usage and configuration.
  Owner: engineering team
- Vendor lock-in to Next.js image solution.
  Mitigation: Document the migration path if the project ever moves away from Next.js, ensuring image assets are stored in a portable format.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all images have meaningful `alt` text for accessibility.
- Consider using a `loader` prop for images hosted on external CDNs.

## Continuation Context


Verify commands:
- Inspect the project's dependency manifest to confirm `next` is listed.
- Run the project's build command and observe image optimization logs.
- Execute the project's test suite to ensure no image-related regressions.

Accept when:
- `next/image` is the primary component used for displaying images in UI components.
- Images are optimized and served efficiently in production builds.
- No significant performance regressions are introduced by image handling.

## Enforcement

- Verified by: Automated linting rules, code reviews, performance monitoring in CI/CD.
- Violation handling: Automated build failures for critical violations, code review comments for minor issues.
- Exception process: Documented exception request process requiring architectural review and approval.