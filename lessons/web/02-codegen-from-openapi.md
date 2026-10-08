# Web Lesson 02: Generate Client from OpenAPI

## Outcomes

- Remove manual API typing drift.
- Align frontend contracts with backend OpenAPI.

## Tasks

1. Select OpenAPI code generation tool for TypeScript.
2. Configure generation output location inside `books-web`.
3. Add generation command to CI and local developer workflow.
4. Prevent manual edits to generated files.
5. Add validation check that generated client is up to date.

## Acceptance Criteria

- Client generation is reproducible.
- API schema changes are visible in pull request diffs.
- Frontend compiles using generated types.
