# Backend Lesson 02: Database and Migrations

## Outcomes

- Model core entities for MVP.
- Set up migration workflow.
- Validate schema evolution process.

## MVP Tables

- `users`
- `books`
- `reading_sessions`
- `shelves` (or enum + relation table)
- `book_notes` (optional in MVP, required post-MVP)

## Tasks

1. Pick ORM + migration tooling (SQLAlchemy + Alembic recommended).
2. Define initial schema with constraints and indexes.
3. Add created/updated timestamps.
4. Generate and run initial migration.
5. Add DB bootstrap docs for local and CI.

## Test-First Checkpoint

- Write integration tests for one persisted flow:
  - create book
  - update status
  - fetch by user

## Acceptance Criteria

- Local DB migration is reproducible from empty database.
- Schema supports book progress and session logging.
- DB tests run in CI with isolated test database.
