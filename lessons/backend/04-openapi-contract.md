# Backend Lesson 04: OpenAPI as Contract

## Outcomes

- Treat OpenAPI spec as published integration contract.
- Version APIs intentionally.
- Prevent undocumented endpoint drift.

## Tasks

1. Ensure all routes, models, and auth schemes are represented in generated OpenAPI.
2. Add API versioning strategy (`/v1` route prefix recommended).
3. Add examples and descriptions for key endpoints.
4. Export OpenAPI artifact in CI.
5. Add contract change review checklist to pull requests.

## Contract Rules

- Breaking changes require version bump and migration notes.
- Response schema changes must be explicit in changelog.
- Web and Android integration should use generated/validated types.

## Acceptance Criteria

- OpenAPI file can be generated in CI and attached as artifact/release asset.
- Contract review is part of normal pull request workflow.
- Consumers can regenerate clients from published spec without manual edits.
