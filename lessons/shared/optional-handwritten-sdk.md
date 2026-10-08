# Shared Lesson: Optional Handwritten SDK

## When To Build It

Use this only if generated clients are insufficient and repeated logic appears in multiple consumers.

## Scope Boundaries

- Keep SDK focused on stable shared behavior.
- Avoid framework-specific UI concerns.
- Prefer thin wrappers over large abstractions.

## Packaging Options

- TypeScript package published to npm registry.
- Python helper package published to PyPI (if needed for scripts/tools).

## Versioning Rules

- Semantic versioning required.
- Changelog required for each release.
- Deprecate APIs before removal where possible.

## Acceptance Criteria

- SDK has clear ownership and release cadence.
- Consumers can pin versions and upgrade intentionally.
- SDK does not recreate backend business logic.
