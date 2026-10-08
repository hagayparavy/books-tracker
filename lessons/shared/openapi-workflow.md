# Shared Lesson: OpenAPI Publishing Workflow

## Goal

Enable independent repositories to integrate safely via published API contracts.

## Workflow

1. `books-api` CI generates `openapi.json`.
2. CI uploads spec as:
   - workflow artifact, and/or
   - GitHub Release asset tied to API version tag.
3. `books-web` and `books-android` consume published spec by URL or downloaded artifact.
4. Client generation runs in each consumer repo CI.

## Independence Rule

- No `../books-api` local path dependency in consumer repos.
- Contract integration must work from clean checkout on any machine.

## Versioning Policy

- Breaking API changes require major version update and migration notes.
- Non-breaking additions use minor version updates.

## Acceptance Criteria

- Consumer repos can regenerate clients without local sibling folders.
- API contract provenance is traceable to tagged backend release.
