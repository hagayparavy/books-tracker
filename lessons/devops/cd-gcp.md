# DevOps Lesson: CD on GCP

## Outcomes

- Deploy backend consistently to cloud.
- Run safe migration and rollback procedures.

## Recommended Path

- Deploy API container to Cloud Run.
- Use Cloud SQL PostgreSQL for persistent data.
- Use Secret Manager for runtime configuration.

## Deployment Flow

1. Build and scan container image.
2. Push image to Artifact Registry.
3. Deploy revision to Cloud Run.
4. Run database migration step safely.
5. Verify health checks and smoke tests.

## Rollback Strategy

- Keep previous stable Cloud Run revision available.
- Use migration strategy compatible with rollback where possible.
- Document emergency procedure and ownership.

## Acceptance Criteria

- Deployment can run from GitHub Actions with OIDC auth.
- Staging and production environments are separated.
- Post-deploy health verification is automated.
