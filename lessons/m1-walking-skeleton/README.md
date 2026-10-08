# Module 1 — Walking Skeleton

> **Repo:** `books-api` · **Total time:** ~20–25 h · **Ends with:** `books-api v0.1.0` live on Cloud Run

## Goal

Build the thinnest possible slice of the backend that goes through **every** stage of the delivery
pipeline: code → tests → CI → container → cloud → tagged release. It does almost nothing as a product
(a health endpoint and configuration), and that's deliberate.

This is called a *walking skeleton*. Teams build one first because integration and delivery problems
(auth to the cloud, secrets, Docker quirks, branch protection) are cheapest to solve while the codebase is tiny.
From Module 2 on, every feature you add flows through a pipeline that already works, and
"CI/CD" and "releasing" become habits instead of a final chapter.

## What you'll have at the end

- A public `books-api` repo with a clean Python project, linting, typing, and pre-commit hooks
- A test-first habit and a pytest setup you'll extend for the rest of the course
- Environment-driven configuration with a secret flowing from GCP Secret Manager into the app
- CI that blocks merges on lint, type, test, and coverage failures
- A production-style Docker image
- Automatic deploys to Cloud Run using keyless GitHub → GCP authentication (OIDC)
- A release process with semantic versions, a generated changelog, and a practiced rollback

## Lessons

| # | Lesson | Time |
|---|---|---|
| 01 | [Repository bootstrap](01-repo-bootstrap.md) | ~3 h |
| 02 | [TDD your first endpoint](02-tdd-first-endpoint.md) | ~2–3 h |
| 03 | [Configuration and dependency injection](03-config-and-di.md) | ~2–3 h |
| 04 | [CI from day one](04-ci-from-day-one.md) | ~3 h |
| 05 | [Container image](05-container-image.md) | ~3 h |
| 06 | [Deploy to Cloud Run](06-deploy-cloud-run.md) | ~4–6 h |
| 07 | [First release and rollback](07-first-release.md) | ~3 h |

## Costs

Expect roughly **$0–2/month**. Cloud Run scales to zero and has a free tier, Artifact Registry
charges for storage only (you'll add a cleanup policy), and Secret Manager costs cents.
Lesson 06 starts with a **budget alert** before you create anything billable.

## Before you start

- Read [Architecture overview](../../architecture/overview.md) and [ADR-0001](../../architecture/adr-0001-stack.md).
- Create `books-api/` as a subfolder of this workspace. It's its own git repo and is gitignored here.
- Decide which git identity you'll commit with for this personal public project (name + email),
  and set it per-repo if it differs from your global one.
