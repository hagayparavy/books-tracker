# Backend Lesson 03: Google Auth Integration

## Outcomes

- Implement OIDC login flow with Google.
- Map external identity to internal user.
- Issue secure app auth token/session.

## Tasks

1. Register OAuth app and collect client ID/secret in local env.
2. Implement token/code verification endpoint.
3. Create user record on first login, update profile metadata on subsequent logins.
4. Choose auth mode:
   - signed session cookies, or
   - short-lived access token + optional refresh strategy.
5. Add auth middleware/dependency for protected endpoints.

## Security Notes

- Never store raw client secrets in repository.
- Validate token audience, issuer, and expiration.
- Log auth events without logging private token contents.

## Test-First Checkpoint

- Unit test token validation adapter with mocked provider response.
- Integration test protected endpoint rejects unauthenticated requests.

## Acceptance Criteria

- Login flow works end-to-end in local environment.
- Protected routes require valid authentication context.
- Security-sensitive values are sourced from environment/secrets manager.
