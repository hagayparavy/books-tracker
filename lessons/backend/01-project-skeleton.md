# Backend Lesson 01: Project Skeleton

## Outcomes

- Create a clean FastAPI project layout.
- Configure environment-driven settings.
- Establish lint/test/dev commands.

## Suggested Structure

- `app/main.py`
- `app/api/`
- `app/domain/`
- `app/services/`
- `app/db/`
- `tests/`

## Tasks

1. Initialize project using `uv` or Poetry.
2. Add FastAPI app with health endpoint.
3. Add settings object reading from environment.
4. Add dependency-injected config access pattern.
5. Add formatter/linter and baseline test runner.

## Acceptance Criteria

- API starts locally with one command.
- Health endpoint returns success JSON.
- No hard-coded credentials or environment values.
- Test command can run from clean clone.

## Reflection

- Which folder boundaries feel most maintainable?
- What naming conventions will you standardize now?
