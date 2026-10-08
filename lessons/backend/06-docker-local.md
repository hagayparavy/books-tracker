# Backend Lesson 06: Local Docker Workflow

## Outcomes

- Containerize API for consistent local setup.
- Run local DB through Docker Compose.
- Prepare deployment parity mindset.

## Tasks

1. Create API Dockerfile with production-like runtime.
2. Create compose stack for:
   - API container
   - PostgreSQL container
3. Pass environment values via compose env file for local only.
4. Run migrations at startup step or dedicated migration command.
5. Document local lifecycle commands: up, logs, down, reset.

## Operational Checks

- Health endpoint available from containerized API.
- API can connect to DB via compose network.
- Restarting stack preserves data unless volume reset is explicit.

## Acceptance Criteria

- New machine can run API + DB with documented commands.
- No secret values embedded in Dockerfile or committed env files.
- Local and CI run similar command patterns where possible.
