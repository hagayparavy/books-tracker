# Web Lesson 05: Testing Strategy

## Outcomes

- Create confidence for UI and client integration behavior.
- Prevent regressions in critical reading flows.

## Test Layers

- Unit tests for utility and pure view logic.
- Component tests for forms, lists, and interactions.
- Contract-oriented tests with mocked API responses from OpenAPI expectations.

## Tasks

1. Configure test runner (Vitest recommended).
2. Add React Testing Library setup.
3. Add test cases for:
   - library rendering
   - add/edit book forms
   - progress update interactions
   - authentication guard behavior
4. Add CI execution and minimum coverage threshold.

## Acceptance Criteria

- Tests run headless in CI.
- Critical user flows are covered by automated tests.
- Broken contract assumptions fail tests early.
