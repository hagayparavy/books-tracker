# ADR-0001: Initial Stack and Repository Independence

- Status: Accepted (amended 2026-10-08)
- Date: 2026-03-25
- Owners: Project Maintainer

## Context

The project goals require:

- Web and Android clients
- Python backend learning
- Independent repositories with no local filesystem dependency
- Public GitHub-safe workflows with strong secret handling
- A lean course: one cloud provider, depth over breadth

## Decision

Adopt the following baseline:

- Backend: Python + FastAPI
- Database: PostgreSQL
- Web: TypeScript + React (Vite)
- Android: Kotlin + Jetpack Compose + Material 3 (recorded per-repo in `books-android`'s own ADR)
- Contract: OpenAPI as source of truth
- Shared library: `bookmeta` on PyPI (see [ADR-0002](adr-0002-bookmeta-library.md))
- Containerization: Docker for the API (local and Cloud Run)
- Cloud: **GCP only** (Cloud Run, Cloud SQL, Secret Manager, Artifact Registry)

Enforce repository independence:

- No imports from sibling repo paths.
- Integrations only via HTTP APIs, published packages, or published artifacts (e.g. the OpenAPI spec on a GitHub Release).
- CI and deployment pipelines are isolated per repository.

## Alternatives Considered

1. Full monorepo with shared source imports
2. Node/Nest backend only
3. No formal API contract and manual client typing
4. Flutter for Android (single cross-platform UI codebase) — rejected to keep a native Kotlin track
5. Parallel AWS track — rejected to keep the course lean (see Consequences)

## Consequences

### Positive

- Strong portability between machines and CI runners.
- Better long-term maintainability with explicit contracts.
- Direct support for Python upskilling goals.

### Negative

- More CI/CD setup overhead across repositories.
- Requires versioning discipline for API changes.
- **AWS is not covered.** It must be learned in a separate course. The design keeps that cheap:
  the API is a stateless container, config is environment-driven, and secrets are injected at runtime,
  so moving to AWS is mostly a mapping exercise (see the README's "Out of scope" section).

## Implementation Notes

- Publish the OpenAPI artifact in API CI.
- Generate client code for web/Android from OpenAPI.
- Keep secrets out of repository history and configs.
