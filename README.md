# Books Tracker Course Workspace

A documentation-first course for building a multi-repository Books Tracker: a Python API, a responsive
web app, an Android app, and a reusable Python library. All code is written by the student in separate,
independent repositories. This workspace holds the lessons, architecture, design, and course progress.

The original brief: [`Books Tracking Project.md`](Books%20Tracking%20Project.md).

## Course goals

- Build a production-style Books Tracker across web and Android clients.
- Sharpen Python with a FastAPI backend and a published library.
- Work test-first, with CI from day one and a release at the end of every module.
- Keep all repositories independent (no local path coupling).
- Be safe to publish on GitHub: keyless cloud auth, secrets only in a secret manager.

## Repository map

Each repo is a subfolder of this workspace on the author's machine but an **independent git repository**
(gitignored here). They integrate only over HTTP, published packages, or published artifacts.

| Repo | Stack | Module |
|---|---|---|
| `books-api` | Python, FastAPI, PostgreSQL, Docker, Cloud Run | M1, M2 |
| `bookmeta` | Python library on PyPI (ISBN + book metadata) | M3 |
| `books-web` | TypeScript, React, Vite | M4 |
| `books-android` | Kotlin, Jetpack Compose, Material 3 | M5 |

## How to use this course

1. Follow **[lessons/LESSONS-ORDER.md](lessons/LESSONS-ORDER.md)**: modules M0–M6, each ending with a release.
2. Track where you are in **[PROGRESS.md](PROGRESS.md)**.
3. With Claude Code, use the session rhythm:
   - `/course-status` — briefing: where you are and what's next
   - `/lesson-review` — read-only review of your repo against the lesson's acceptance criteria
   - `/session-wrap` — save progress, decisions, and next steps to `PROGRESS.md`

## Documentation map

- **Architecture:** [overview](architecture/overview.md) · [ADR-0001 stack](architecture/adr-0001-stack.md) ·
  [ADR-0002 bookmeta](architecture/adr-0002-bookmeta-library.md) · [ADR template](architecture/adr-template.md)
- **Product and UX:** [product spec](design/product-spec.md) · [UI principles](design/ui-principles.md) ·
  [wireframe checklist](design/wireframe-checklist.md) · [Figma tasks](design/figma-tasks.md)
- **Lessons:** [order](lessons/LESSONS-ORDER.md) · [template](lessons/LESSON-TEMPLATE.md) ·
  [Module 1](lessons/m1-walking-skeleton/README.md)
- **Capstone checklists:** [MVP done](checklists/mvp-done.md) · [public GitHub readiness](checklists/github-public.md)

## Out of scope

To keep the course lean, these are deliberately excluded and left for separate courses:

- **AWS.** The follow-up course should cover the equivalents of what's used here:
  - App Runner or ECS Fargate (≈ Cloud Run)
  - RDS PostgreSQL (≈ Cloud SQL)
  - Secrets Manager (≈ Secret Manager)
  - ECR (≈ Artifact Registry)
  - IAM roles plus GitHub OIDC federation (≈ Workload Identity Federation)
  - CloudWatch (≈ Cloud Logging and Monitoring)

  The design here is portable (stateless container, env-driven config, runtime-injected secrets),
  so that course is mostly a mapping exercise.
- **Deep authentication theory.** Sign-in uses Google as a third-party identity provider; the course covers
  wiring it securely, not building an auth system.
- **iOS.** Android is native Kotlin. Cross-platform UI (Flutter, React Native) would need a new ADR.
