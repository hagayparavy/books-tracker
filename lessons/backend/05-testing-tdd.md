# Backend Lesson 05: Testing and TDD Workflow

## Outcomes

- Establish repeatable TDD loop for endpoint development.
- Combine unit and integration testing for confidence.

## TDD Loop

1. Write failing test for desired behavior.
2. Implement minimal code to pass test.
3. Refactor while keeping tests green.

## Test Pyramid for API

- Unit tests: domain logic and validators.
- Integration tests: repository + database behavior.
- API tests: route behavior, auth boundaries, response contracts.

## Tasks

1. Configure pytest and fixtures for DB and auth context.
2. Add factories/builders for test data.
3. Add at least one end-to-end test for each core use case:
   - add book
   - update progress
   - move shelf/status
   - list books by filter
4. Add coverage reporting in CI.

## Acceptance Criteria

- Tests are deterministic and isolated.
- Minimum target coverage defined and visible in CI.
- At least one failure case test exists per core endpoint.
