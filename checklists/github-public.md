# Public GitHub Readiness Checklist

## Repository Hygiene

- [ ] Add license file.
- [ ] Add `README.md` with architecture and setup details.
- [ ] Add `CONTRIBUTING.md` (optional for solo, useful for future collaborators).
- [ ] Add `SECURITY.md` with vulnerability reporting path.

## Secret Safety

- [ ] `.env` files are gitignored.
- [ ] `.env.example` exists without real secret values.
- [ ] History scan completed for accidental keys/tokens.
- [ ] Secret rotation executed if any exposure occurred.

## CI/CD and Governance

- [ ] Branch protection enabled for main branch.
- [ ] Required checks configured.
- [ ] Dependabot or equivalent update strategy enabled.

## Compliance and Trust

- [ ] Third-party licenses reviewed.
- [ ] Privacy-sensitive data handling documented.
- [ ] Minimal telemetry policy documented (if any telemetry is used).

## Release Visibility

- [ ] Changelog exists and is current.
- [ ] Release tags are consistent.
- [ ] Deployment process and rollback path are documented.
