# Lessons order

The authoritative course sequence. The course is organised in **modules**, and each one ends with a tagged release.
Lessons follow [LESSON-TEMPLATE.md](LESSON-TEMPLATE.md); [Module 1](m1-walking-skeleton/README.md) is the reference.

**Status legend:** ✅ written (new format) · 📝 legacy draft (old format, to be rewritten) · 🆕 to be written

Live progress (what *you've* completed) is tracked in [`PROGRESS.md`](../PROGRESS.md), not here.

---

## M0 — Foundations (course workspace)

| # | Lesson | Status |
|---|---|---|
| 0.1 | [Architecture overview](../architecture/overview.md) | ✅ |
| 0.2 | [ADR-0001: stack and independence](../architecture/adr-0001-stack.md) | ✅ |
| 0.3 | [ADR-0002: `bookmeta` library](../architecture/adr-0002-bookmeta-library.md) | ✅ |
| 0.4 | [ADR template](../architecture/adr-template.md) (reference) | ✅ |
| 0.5 | [Product specification](../design/product-spec.md) | ✅ |
| 0.6 | [UI principles](../design/ui-principles.md) | ✅ |
| 0.7 | [Wireframe checklist](../design/wireframe-checklist.md) | ✅ |
| 0.8 | [Figma tasks](../design/figma-tasks.md) | ✅ |

Design work (0.7–0.8) can run in parallel with Module 1 or 2; it must be done before Module 4.

## M1 — Walking skeleton → `books-api v0.1.0` live

| # | Lesson | Status |
|---|---|---|
| 1.01 | [Repository bootstrap](m1-walking-skeleton/01-repo-bootstrap.md) | ✅ |
| 1.02 | [TDD your first endpoint](m1-walking-skeleton/02-tdd-first-endpoint.md) | ✅ |
| 1.03 | [Configuration and dependency injection](m1-walking-skeleton/03-config-and-di.md) | ✅ |
| 1.04 | [CI from day one](m1-walking-skeleton/04-ci-from-day-one.md) | ✅ |
| 1.05 | [Container image](m1-walking-skeleton/05-container-image.md) | ✅ |
| 1.06 | [Deploy to Cloud Run](m1-walking-skeleton/06-deploy-cloud-run.md) | ✅ |
| 1.07 | [First release and rollback](m1-walking-skeleton/07-first-release.md) | ✅ |

## M2 — API MVP → `books-api v1.0.0` in production

| # | Lesson | Status | Draws on |
|---|---|---|---|
| 2.01 | Database, migrations, and DB test fixtures (Postgres in Compose, SQLAlchemy 2.0, Alembic) | 📝 | [backend/02](backend/02-database-migrations.md), [backend/05](backend/05-testing-tdd.md), [backend/06](backend/06-docker-local.md) |
| 2.02 | Google sign-in and app sessions (kept light) | 📝 | [backend/03](backend/03-auth-google.md) |
| 2.03 | Books and shelves (test-first CRUD) | 🆕 | product spec |
| 2.04 | Reading progress and sessions | 🆕 | product spec |
| 2.05 | OpenAPI contract, versioning, and publishing the spec | 📝 | [backend/04](backend/04-openapi-contract.md), [shared/openapi-workflow](shared/openapi-workflow.md) |
| 2.06 | Staging and production (Cloud SQL, prod environment, migrations in the deploy, `v1.0.0`) | 📝 | [devops/cd-gcp](devops/cd-gcp.md), [devops/release-process](devops/release-process.md) |

## M3 — Library: `bookmeta` → `bookmeta v0.1.0` on PyPI

| # | Lesson | Status |
|---|---|---|
| 3.01 | Library design and packaging (public API, `pyproject.toml`, build backend, typed package) | 🆕 |
| 3.02 | ISBN core, test-first (plus property-based testing with Hypothesis) | 🆕 |
| 3.03 | Metadata providers (HTTP client, provider abstraction, mocking, resilience) | 🆕 |
| 3.04 | Docs, CI matrix, and release via PyPI Trusted Publishing (TestPyPI first) | 🆕 |
| 3.05 | Adopt `bookmeta` in `books-api`: add a book by ISBN | 🆕 |

## M4 — Web → `books-web v1.0.0`

| # | Lesson | Status | Draws on |
|---|---|---|---|
| 4.01 | Vite + React setup, Vitest + Testing Library, CI | 📝 | [web/01](web/01-vite-react-setup.md), [web/05](web/05-testing.md), [devops/ci-per-repo](devops/ci-per-repo.md) |
| 4.02 | Generated client from the published OpenAPI spec | 📝 | [web/02](web/02-codegen-from-openapi.md) |
| 4.03 | Google sign-in button and route guard (light) | 🆕 | — |
| 4.04 | Server state and forms | 📝 | [web/03](web/03-state-and-forms.md) |
| 4.05 | Responsive layout and optional PWA | 📝 | [web/04](web/04-responsive-pwa.md) |
| 4.06 | Hosting, deploy, and release | 🆕 | [devops/release-process](devops/release-process.md) |

## M5 — Android → `books-android v1.0.0` (APK on GitHub Release)

| # | Lesson | Status | Draws on |
|---|---|---|---|
| 5.00 | Framework decision recorded (Kotlin + Compose + Material 3), plus CI | 📝 | [android/00](android/00-choose-framework.md), [devops/ci-per-repo](devops/ci-per-repo.md) |
| 5.01 | Project setup and auth | 📝 | [android/01](android/01-project-auth.md) |
| 5.02 | API integration layer | 📝 | [android/02](android/02-api-layer.md) |
| 5.03 | Core UI screens | 📝 | [android/03](android/03-ui-screens.md) |
| 5.04 | Signed release build and GitHub Release | 🆕 | [devops/release-process](devops/release-process.md) |

## M6 — Hardening and going public

| # | Lesson | Status | Draws on |
|---|---|---|---|
| 6.01 | Observability: structured logs, request IDs, an uptime check, an error alert | 🆕 | architecture overview |
| 6.02 | Production rollback drill with a database (expand/contract migrations) | 🆕 | [devops/cd-gcp](devops/cd-gcp.md) |
| 6.03 | [MVP definition of done](../checklists/mvp-done.md) | ✅ | — |
| 6.04 | [Public GitHub readiness](../checklists/github-public.md) | ✅ | — |

---

## Removed legacy files

Absorbed into Module 1 and deleted (recoverable from git history, commit `3a384db`):
`backend/01-project-skeleton.md` → 1.01–1.03 · `devops/secrets-and-config.md` → 1.03, 1.06 ·
`shared/optional-handwritten-sdk.md` → replaced by M3 (`bookmeta`).

## Notes

- Every repo is a subfolder of this workspace on your machine but an **independent git repo**. No repo
  may reference another by local path (see [ADR-0001](../architecture/adr-0001-stack.md)).
- CI and releases aren't a separate phase: each module's first lesson sets up CI for its repo,
  and each module ends with a release.
