# ADR-0002: `bookmeta` as the Course's Reusable Library

- Status: Accepted
- Date: 2026-10-08
- Owners: Project Maintainer

## Context

The brief asks to learn "how to build a library that can be used in other codebases".
The legacy plan only had an optional handwritten SDK, which in practice would never be needed
because the clients use generated OpenAPI code. The library must be:

- Genuinely reusable outside Books Tracker (otherwise it's just a module in the wrong place)
- Free of Books Tracker business logic
- Good Python practice
- Consumed by one of the course repos, to prove the "integrate via published packages" rule

## Decision

Build **`bookmeta`**, a small, typed Python library published to PyPI:

- ISBN-10 / ISBN-13 validation, normalization, and conversion
- Book metadata lookup by ISBN from public sources (Open Library first, Google Books optional),
  returning a normalized, provider-independent model

`books-api` consumes it as a pinned dependency from PyPI, which enables an "add book by ISBN" feature.

It lives in its own repository (`bookmeta`) with its own CI, versioning, and releases.
Publishing uses **PyPI Trusted Publishing** (OIDC from GitHub Actions), so no API tokens are stored.

## Alternatives Considered

1. Publish the generated TypeScript API client to npm — useful, but mostly generated code, so less to learn.
   Kept as a stretch goal.
2. A shared domain-model package across repos — rejected: couples repos and duplicates backend logic.
3. No library — fails a stated course goal.

## Consequences

### Positive

- Teaches packaging (`pyproject.toml`, build backends), public API design, semver, deprecation,
  test matrices across Python versions, docs, and release automation.
- Adds product value (faster book entry) without bloating the API.

### Negative

- One more repository with CI and releases to maintain.
- Depends on third-party public APIs that can change or rate-limit; the library must handle that gracefully.

## Implementation Notes

- Module 3 of the course (see `lessons/LESSONS-ORDER.md`).
- Publish to TestPyPI first, then PyPI.
- `books-api` adopts it in a dedicated lesson after Module 3.
