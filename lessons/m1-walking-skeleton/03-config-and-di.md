# M1.03 — Configuration and dependency injection

> **Repo:** `books-api` · **Time:** ~2–3 h · **Prerequisites:** M1.02

## Why

**Configuration is the seam between your code and the world.** The same image must run locally, in CI,
in staging, and in production, and only configuration differs. The
[Twelve-Factor](https://12factor.net/config) rule is to read config from **environment variables**,
validate it once at startup, and fail fast and loudly if anything is missing. This is also the core of
your "public repo, no leaked secrets" requirement: code knows the *names* of settings, never the *values*.

**pydantic-settings** gives you a typed settings class. Each field is read from an environment variable,
coerced to the right type, and validated. Sensitive fields use `SecretStr`, which prints as `**********`
in logs and reprs, so an accidental log line doesn't leak the value.

**Dependency injection, FastAPI style.** Route functions declare what they need using `Depends(...)`, and
FastAPI resolves it per request. There's no container framework: dependencies are plain functions.
The payoff is in tests. `app.dependency_overrides` lets a test swap a real dependency (settings, later a
DB session or the Google token verifier) for a fake, without patching imports or touching real env vars.

**A common Python trap:** a module-level `settings = Settings()` runs at *import time*. Tests then can't
control it, and an import can crash because a variable is missing in an unrelated context. Construct
settings through a function (cached), and reach it through DI.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Env var naming | bare names / prefixed | **Prefix `BOOKS_API_`**: no collisions on shared hosts or in CI |
| Settings lifetime | build per request / cache once | **Cache once** (`functools.lru_cache` on the getter); override in tests |
| Environment indicator | free string / enum | **Enum**: `local`, `test`, `staging`, `production` |
| `.env` file support | always / local only / never | **Local only**: in the cloud, real env vars and Secret Manager are the source |
| Version source | hard-coded / package metadata / env var | **Package metadata** (`importlib.metadata`) for the version, plus a `BOOKS_API_GIT_SHA` env var set at build time |

## Tests first

1. Settings load from environment variables with the expected types (e.g. log level, environment enum).
2. A **missing required setting** makes startup fail with a clear validation error naming the variable.
3. An invalid value (e.g. `BOOKS_API_ENVIRONMENT=banana`) fails validation.
4. A `SecretStr` field never appears in its plain value when the settings object is printed or serialised.
5. `GET /health` now also returns `version`, `git_sha`, and `environment`, and **no** secret values.
6. A test can override settings through `dependency_overrides` and see the overridden values in `/health`,
   without setting real environment variables.

Use the `monkeypatch` fixture for tests 1–3 so the environment is restored after each test.

## Tasks

1. **Add pydantic-settings.** Create a `Settings` class in e.g. `books_api/config.py` with:
   - `environment` (enum)
   - `log_level`
   - `git_sha` (optional, default `"unknown"`)
   - `app_secret` (`SecretStr`, **required**): a placeholder secret whose only job for now is to prove the
     secret pipeline end to end in M1.06. Later it will sign session tokens.
2. **Expose `get_settings()`** as a cached function, and use it as a FastAPI dependency.
3. **Pass settings into the factory:** `create_app()` accepts optional settings, so tests and scripts can
   build an app with explicit configuration.
4. **Extend the health response** with version, git SHA, and environment. Update the response model and tests.
5. **`.env` for local dev:** enable `.env` loading in the settings config, fill `.env.example` with every
   variable (fake placeholder values plus a comment each), and confirm `.env` is gitignored.
6. **Test fixtures:** update `conftest.py` so the test app always gets explicit test settings
   (environment `test`, a dummy secret). The suite must pass with **no `.env` file present**.
7. **Document** a configuration table in the README: variable, type, required?, default, description.

## Acceptance criteria

- [ ] A typed `Settings` class validates all config at startup; a missing or invalid value fails fast with a clear error
- [ ] Secrets use `SecretStr` and never appear in logs, reprs, or responses
- [ ] Settings are reached via `Depends(get_settings)`, with no module-level instance
- [ ] The test suite runs green with no `.env` file and no real env vars set
- [ ] At least one test uses `dependency_overrides`
- [ ] `/health` reports version, git SHA, and environment
- [ ] `.env.example` lists every variable; `.env` is gitignored; the README has a config table

## Stretch

- Read how pydantic-settings orders its sources (init args, env, dotenv, secrets dir) and when you'd use a
  custom source. This is the hook for reading from Secret Manager directly if you ever need it.
- Add a `validate-config` runner command that builds settings from the current env and exits non-zero on error.
  It's handy in deploy pipelines.

## References

- Twelve-Factor App — Config: https://12factor.net/config
- pydantic-settings: https://docs.pydantic.dev/latest/concepts/pydantic_settings/
- Pydantic `SecretStr`: https://docs.pydantic.dev/latest/api/types/#pydantic.types.SecretStr
- FastAPI — settings and environment variables: https://fastapi.tiangolo.com/advanced/settings/
- FastAPI — dependencies: https://fastapi.tiangolo.com/tutorial/dependencies/
- FastAPI — testing with dependency overrides: https://fastapi.tiangolo.com/advanced/testing-dependencies/
- pytest — monkeypatch: https://docs.pytest.org/en/stable/how-to/monkeypatch.html
