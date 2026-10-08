# DevOps Lesson: Release Process

## Outcomes

- Release each repository with predictable versioning.
- Document changes and reduce rollback risk.

## Versioning

- Use semantic versioning per repository.
- Maintain changelog entries per release.
- Tag releases consistently.

## Release Checklist

1. All CI checks green.
2. Contract compatibility reviewed (for API changes).
3. Changelog updated.
4. Release tag created.
5. Deployment completed and verified.
6. Rollback plan confirmed.

## Conventions

- Keep release cadence frequent and small.
- Prefer conventional commit style for automated changelog tooling.
- Publish release notes with migration notes when needed.

## Acceptance Criteria

- Release can be executed from clean branch with documented steps.
- Consumers can identify compatibility impact from release notes.
- Rollback drill has been practiced at least once.
