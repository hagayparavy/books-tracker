# M1.05 — Container image

> **Repo:** `books-api` · **Time:** ~3 h · **Prerequisites:** M1.04

## Why

Earlier you weren't sure whether Docker fits this project. For the API it fits well: **the container image
is the deployable artifact.** Cloud Run runs containers, CI builds the exact image that will be deployed,
and locally you can run the same thing. The web and Android apps won't need Docker for development, and that's fine.

**What makes a production-grade Python image:**

- **Multi-stage build.** A *builder* stage installs dependencies with uv (and any compilers). A slim
  *runtime* stage copies only the resulting virtualenv and your code. The result is smaller and has less
  attack surface.
- **Layer caching order.** Copy the lockfile and install dependencies *before* copying source code, so a
  code change doesn't reinstall everything.
- **Non-root user.** If the process is compromised, it isn't root inside the container.
- **No secrets in the image.** Image layers are permanent and inspectable (`docker history`). Config and
  secrets arrive at *runtime* as env vars, which you already built for in M1.03.
- **Respect `PORT`.** Cloud Run tells your container which port to listen on via the `PORT` env var.
- **One process per container.** On Cloud Run you scale by adding instances, not by adding workers inside
  one container, so start with a single uvicorn worker.
- **Build-time metadata.** Pass the git SHA as a build argument so `/health` can report what's running.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Base image | `python:<ver>-slim` / distroless / uv's images | **`python:<ver>-slim`** for runtime, the uv binary copied in for the builder |
| Server command | `fastapi run` / `uvicorn` directly | Either. **`fastapi run`** reads clearly; know the uvicorn flags it implies. |
| Workers | 1 / many | **1** on Cloud Run; scaling happens at the instance level |
| Image scanning | none / Trivy / Docker Scout | **Trivy** in CI, failing on CRITICAL vulnerabilities that have a fix |
| Local orchestration | `docker run` / Compose | **Compose** (`compose.yaml`). Only the API for now; Postgres joins in Module 2. |

## Tests first

These are container-level behaviours. Script them as a `just smoke` command and later as a CI step:

1. The image builds from a clean clone with no `.env` present.
2. A container started with the required env vars answers `GET /health` with 200, and reports the git SHA
   passed at build time.
3. A container started **without** the required secret exits immediately with a clear validation error
   (proving fail-fast works in the real runtime).
4. The process runs as a **non-root** user.
5. The container listens on whatever `PORT` it's given, not a hard-coded port.

## Tasks

1. **Write a multi-stage `Dockerfile`** following uv's Docker integration guide. Install from the lockfile
   in a frozen or locked mode, without dev dependencies.
2. **Write a `.dockerignore`** that excludes `.git`, `.venv`, caches, tests (unless you deliberately want
   them), and above all **`.env`**.
3. **Add a git SHA build arg** and surface it as `BOOKS_API_GIT_SHA` in the runtime environment.
4. **Write `compose.yaml`** for local use: build the API, map a port, and load env from your local `.env` file.
5. **Add runner commands** `docker-build`, `up`, `down`, `logs`, and `smoke`, and document them.
6. **Add a CI job** that builds the image (no push yet), runs the smoke checks against it, and scans it with Trivy.
7. **Check image size and layers** (`docker image ls`, `docker history`). Note the size in your PR
   description, and try one change to reduce it.

## Acceptance criteria

- [ ] Multi-stage `Dockerfile`; the runtime stage has no build tools and no dev dependencies
- [ ] The container runs as non-root and listens on `$PORT`
- [ ] No secrets or `.env` in the image; `.dockerignore` excludes them
- [ ] `/health` in the container reports the build's git SHA
- [ ] `compose.yaml` plus documented up/down/logs commands work from a clean clone
- [ ] CI builds the image, runs the smoke checks, and runs a vulnerability scan
- [ ] Changing only source code rebuilds without reinstalling dependencies (verify the cache hit)

## Stretch

- Add a `HEALTHCHECK` for local Compose use, and learn why Cloud Run ignores it (it has its own probes).
- Compare image sizes with a distroless runtime base. Is the debugging trade-off worth it?
- Use BuildKit cache mounts for uv's cache to speed up rebuilds.

## References

- uv — Docker integration: https://docs.astral.sh/uv/guides/integration/docker/
- FastAPI — deploying with Docker: https://fastapi.tiangolo.com/deployment/docker/
- Docker — build best practices: https://docs.docker.com/build/building/best-practices/
- Docker — multi-stage builds: https://docs.docker.com/build/building/multi-stage/
- Compose file reference: https://docs.docker.com/reference/compose-file/
- Cloud Run — container runtime contract: https://cloud.google.com/run/docs/container-contract
- Trivy: https://trivy.dev/
