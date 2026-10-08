# DevOps Lesson: Secrets and Configuration

## Outcomes

- Separate configuration from code safely.
- Keep public repositories free of secrets.

## Local Development Rules

- Use `.env` only for local development.
- Commit `.env.example`, never `.env`.
- Keep per-repo config explicit and documented.

## Cloud Secrets Strategy

- GCP: Secret Manager for runtime secrets.
- AWS track: Secrets Manager equivalent.
- Inject secrets at runtime, not build-time when possible.

## GitHub Actions Security

- Prefer OIDC federation to cloud provider.
- Avoid long-lived static cloud keys in GitHub secrets.
- Restrict workflow permissions by default.

## Acceptance Criteria

- No API keys or private credentials in git history.
- Secrets rotation process documented.
- New contributors can configure local env from example files only.
