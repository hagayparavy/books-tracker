# M1.02 — TDD your first endpoint

> **Repo:** `books-api` · **Time:** ~2–3 h · **Prerequisites:** M1.01

## Why

TDD works best when practiced on something trivial first, so the loop becomes muscle memory before the
problems get hard. A health endpoint is ideal: the behaviour is obvious, so you can focus on the
**red → green → refactor** rhythm and on learning pytest.

**pytest for a Jest user:**

| Jest | pytest |
|---|---|
| `expect(x).toBe(y)` | plain `assert x == y`. pytest rewrites asserts to show rich diffs. |
| `beforeEach` / setup | **fixtures**: functions that provide a value, requested by naming them as test parameters. They compose and have scopes (function, module, session). |
| `test.each` | `@pytest.mark.parametrize` |
| shared setup file | `conftest.py`: fixtures there are auto-discovered by tests in that directory and below |
| `jest.mock` | `monkeypatch` fixture, or `unittest.mock`. In FastAPI, prefer **dependency overrides** (next lesson). |

**FastAPI testing:** FastAPI's `TestClient` (built on httpx) calls your app in-process, with no server
or network involved. Requests take microseconds, so API-level tests are cheap enough to write a lot of them.

**App factory:** instead of a module-level `app = FastAPI()`, expose a `create_app()` function.
Each test can then build a fresh, isolated app, and configuration can be injected at creation time
(this pays off in M1.03 and in M2 when databases appear).

**Liveness vs readiness:** a *liveness* check answers "is the process up?" A *readiness* check answers
"can it serve traffic?", meaning things like whether the DB is reachable. Today you build liveness;
readiness arrives with the database in Module 2.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| App construction | module-level instance / app factory | **App factory** (`create_app()`) |
| Test layout | flat `tests/` / `tests/unit` + `tests/api` | **Split by layer**: `tests/unit/`, `tests/api/` (integration added in M2) |
| Health path | `/health` / `/healthz` / `/v1/health` | **`/health`**, unversioned, because it's operational and not part of the product contract |
| Commit granularity during TDD | squash at the end / commit each red and green | **Commit each step locally** while learning. Your history becomes a record of the loop. |

## Tests first

Write these **before** any application code, watch each one fail for the *right reason*, then make it pass:

1. `GET /health` returns status **200**.
2. The body is JSON with `status` equal to `"ok"`.
3. The response content type is `application/json`.
4. An unknown path returns **404** with a JSON body (FastAPI's default — the test documents the expectation).
5. Two apps created by `create_app()` are independent objects. This guards the factory pattern.

"Fails for the right reason" means the first run should fail because the route doesn't exist, not
because of an import error or a typo in the test.

## Tasks

1. **Add FastAPI** as a runtime dependency, using the extra that includes the `fastapi` CLI and uvicorn.
   Add **httpx** to the dev group if your FastAPI version needs it for `TestClient`.
2. **Configure pytest in `pyproject.toml`:** test paths, strict markers, and `-ra` for a useful summary.
3. **Create a `client` fixture in `tests/conftest.py`** that builds an app via the factory and yields a `TestClient`.
4. **Run the TDD loop** for each behaviour above, one at a time:
   - **Red:** write the test and run it. It fails.
   - **Green:** write the *minimum* code to pass.
   - **Refactor:** clean up names and structure. Tests stay green.
5. **Organise the code:** put the route in a router module (e.g. `books_api/api/health.py`) that the factory
   includes. Don't put routes in the factory itself; you'll have many routers soon.
6. **Add a typed response model** for the health payload (a Pydantic model). That also makes it appear
   correctly in the OpenAPI docs.
7. **Add coverage** with pytest-cov. Make `just test` report coverage, and set a starting threshold
   (e.g. 90%) that fails the run if missed.
8. **Run the dev server** with the `fastapi dev` command and open `/docs`. Notice that the OpenAPI UI
   was generated from your code. This is the seed of the contract-first work in Module 2.
9. Add a `just dev` command and document it in the README.

## Acceptance criteria

- [ ] `create_app()` exists, and the dev server runs via `just dev`
- [ ] All five behaviours have tests in `tests/api/`, and they pass
- [ ] The health route lives in its own router module, with a Pydantic response model
- [ ] pytest config lives in `pyproject.toml`; a shared `client` fixture lives in `conftest.py`
- [ ] Coverage is reported, and the run fails below the configured threshold
- [ ] `/docs` shows the health endpoint with its response schema
- [ ] Git history shows the red → green → refactor rhythm, at least for the first behaviour

## Stretch

- Use `parametrize` to check that several unknown paths all return 404 JSON.
- Read about pytest fixture **scopes** and decide which scope your `client` fixture should have, and why.
- Try `pytest --lf` (last failed) and `-x` (stop at first failure) to tighten your loop.

## References

- FastAPI — first steps: https://fastapi.tiangolo.com/tutorial/first-steps/
- FastAPI — testing: https://fastapi.tiangolo.com/tutorial/testing/
- FastAPI — bigger applications (routers): https://fastapi.tiangolo.com/tutorial/bigger-applications/
- pytest — fixtures: https://docs.pytest.org/en/stable/how-to/fixtures.html
- pytest — parametrize: https://docs.pytest.org/en/stable/how-to/parametrize.html
- pytest-cov: https://pytest-cov.readthedocs.io/
