# DevOps Lesson: CI Per Repository

## Outcomes

- Build independent CI pipelines for each repository.
- Enforce quality gates before merge.

## Baseline Pipelines

### API

- lint
- tests (unit + integration)
- build container
- publish OpenAPI artifact

### Web

- lint
- tests
- build
- optional preview deployment

### Android

- static checks/lint
- unit tests
- debug/release build

## Policy

- Branch protection requires green checks.
- Cache dependencies for speed but keep lockfiles authoritative.
- Keep workflow logic minimal and readable.

## Acceptance Criteria

- Each repo can be validated independently in CI.
- Failing tests block merge by policy.
- CI badges and status are visible in each repo README.
