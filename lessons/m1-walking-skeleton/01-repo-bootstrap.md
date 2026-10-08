# M1.01 — Repository bootstrap

> **Repo:** `books-api` · **Time:** ~3 h · **Prerequisites:** Module 0 (architecture + design docs)

## Why

The first hour of a repository sets habits that are expensive to change later: how dependencies are
pinned, how code is laid out, what runs before every commit, and whether secrets can leak. Doing this
deliberately is the difference between a side project and a codebase you'd show at a meetup.

**The modern Python toolchain, mapped to what you already know:**

| TypeScript world | Python world (this course) |
|---|---|
| `package.json` | `pyproject.toml` (the standard project file, PEP 621) |
| `pnpm-lock.yaml` | `uv.lock` |
| `nvm` / `.nvmrc` | `uv` manages Python versions too; `.python-version` |
| `pnpm` | **uv** — resolver, installer, virtualenv manager, and runner in one fast tool |
| ESLint + Prettier | **Ruff** — linter *and* formatter, replaces flake8/isort/black |
| `tsc --noEmit` | **mypy** or **pyright** — static type checking on type hints |
| husky + lint-staged | **pre-commit** framework |

Python has no `node_modules` per project by default. Instead each project gets a **virtual environment**
(`.venv/`), and uv creates and syncs it from the lockfile. `uv run <cmd>` runs a command inside it,
so you rarely "activate" anything.

**Layout:** prefer the **src layout** (`src/books_api/...`) over a top-level `app/` folder. With src layout,
tests import the *installed* package rather than whatever happens to be on the current path. That
catches packaging mistakes early, and it's the layout you'll need for `bookmeta` in Module 3 anyway.

**Public from day one:** making the repo public now forces secret discipline from the very first commit,
and it gets you free GitHub Actions minutes and free secret scanning.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Package manager | uv / Poetry / pip + venv | **uv**: fastest, one tool, now the de-facto standard |
| Layout | `src/books_api/` / flat `app/` | **src layout** (reason above) |
| Type checker | mypy (strict) / pyright / ty | **mypy in strict mode** — most widespread; pyright is fine if you prefer it |
| Ruff rules | defaults only / curated set | **Curated**: start with defaults plus bugbear, pyupgrade, isort, simplify, and naming rules; document why |
| Command runner | documented `uv run` commands / Makefile / `just` | **`just`** (or a Makefile). One-word commands (`just test`, `just lint`) that CI reuses too |
| Commit convention | free-form / Conventional Commits | **Conventional Commits**: required for automated changelogs in lesson 07 |
| License | MIT / Apache-2.0 | **MIT** for simplicity, or Apache-2.0 if you want the explicit patent grant |
| Python version | 3.12 / 3.13 / 3.14 | **Latest stable your dependencies support**, pinned in `.python-version` |

Record the toolchain choices as **`docs/adr/0001-python-toolchain.md`** in `books-api`, using the
[ADR template](../../architecture/adr-template.md).

## Tests first

There's no behaviour yet, so this lesson's "test" is that the toolchain itself works:

1. From a fresh clone, one documented command installs everything, and the test command runs and passes
   with a single smoke test asserting that the `books_api` package imports.
2. A commit containing a lint error or a fake secret is **rejected** by the pre-commit hooks.
   Try both deliberately to prove it, then discard them.

## Tasks

1. **Create the repo.** Create `books-api/` under this workspace, run `git init`, and set your per-repo git
   identity if needed. Create the public GitHub repository and connect the remote.
2. **Initialise the project with uv** as a packaged application with src layout. Pin the Python version.
   Confirm `pyproject.toml`, `uv.lock`, and `.python-version` exist, and commit all three.
3. **Add dev tooling in a dependency group** (not as runtime dependencies): pytest, ruff, and your type checker.
   Learn the difference between `[project.dependencies]` and `[dependency-groups]`.
4. **Configure Ruff and the type checker in `pyproject.toml`.** One config file, no scattered dotfiles.
   Enable strict typing from the start; it's painful to retrofit later.
5. **Add pre-commit hooks:** ruff lint (with autofix), ruff format, the type checker, a secret scanner
   (**gitleaks**), and basic hygiene hooks (trailing whitespace, end-of-file, large files, merge conflicts).
6. **Add the command runner** with at least: `install`, `lint`, `format`, `typecheck`, `test`, `check`
   (runs all of them). These names are what `/lesson-review` and CI will use.
7. **Write the repo hygiene files:**
   - `.gitignore` (Python, `.venv`, `.env`, caches)
   - `.env.example` (empty for now)
   - `LICENSE`
   - `README.md` with: one-paragraph purpose, prerequisites, **Getting started** (clone → install → test),
     and a **Commands** section listing every runner command
8. **Turn on GitHub's safety nets:** Secret scanning with **push protection**, and private vulnerability
   reporting (Settings → Code security).
9. **Write the smoke test**, run `just check`, and commit using Conventional Commits.

## Acceptance criteria

- [ ] `books-api` is a public GitHub repo with `pyproject.toml`, `uv.lock`, and `.python-version` committed
- [ ] Source lives under `src/books_api/`; tests under `tests/`
- [ ] Ruff and the type checker are configured in `pyproject.toml`; strict typing is enabled
- [ ] Pre-commit hooks run ruff, the type checker, gitleaks, and hygiene checks, and they block bad commits
- [ ] One-word runner commands exist for install, lint, format, typecheck, test, and check, and the README documents them
- [ ] A fresh clone gets to green tests with only the README's commands
- [ ] `docs/adr/0001-python-toolchain.md` records your choices and the reasons
- [ ] Secret scanning push protection is enabled
- [ ] Commit history follows Conventional Commits

## Stretch

- Add `.editorconfig` so editors agree on whitespace across all your future repos.
- Configure your IDE to use the project's `.venv` interpreter and Ruff as formatter on save.
- Read how uv resolves for multiple platforms, and what the `uv.lock` "universal lockfile" means.

## References

- uv — projects guide: https://docs.astral.sh/uv/guides/projects/
- uv — dependency groups: https://docs.astral.sh/uv/concepts/projects/dependencies/
- Python Packaging Guide — src vs flat layout: https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/
- Ruff configuration: https://docs.astral.sh/ruff/configuration/
- mypy configuration: https://mypy.readthedocs.io/en/stable/config_file.html
- pre-commit: https://pre-commit.com/
- gitleaks: https://github.com/gitleaks/gitleaks
- just: https://just.systems/man/en/
- Conventional Commits: https://www.conventionalcommits.org/
- GitHub push protection: https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection
