# M1.04 — CI from day one

> **Repo:** `books-api` · **Time:** ~3 h · **Prerequisites:** M1.03

## Why

Pre-commit hooks are a courtesy: anyone can skip them with `--no-verify`. **CI is the referee.**
It runs the same checks on a clean machine for every pull request, and with branch protection,
nothing reaches `main` unless the referee agrees. Even solo, this matters: it's what makes
"main is always deployable" true, and M1.06 will deploy every merge automatically.

**Conventions worth adopting now:**

- **Lockfile is authoritative.** CI installs with `uv sync --locked`, so if `uv.lock` is out of date
  relative to `pyproject.toml`, the build fails instead of silently resolving something new.
- **CI calls the same commands you run locally** (`just lint`, `just test`). The workflow file stays
  thin, and "works on my machine" has fewer places to hide.
- **Least privilege.** The automatic `GITHUB_TOKEN` should get read-only permissions by default.
  Jobs that need more ask for it explicitly.
- **Supply-chain hygiene.** Third-party actions run with access to your repo. Pin them to a **full
  commit SHA** rather than a moving tag like `v4`, and let Dependabot propose updates.
- **Fast feedback.** Cache dependencies, run independent jobs in parallel, and cancel superseded runs
  on the same branch.

**Rulesets vs classic branch protection:** GitHub's newer **rulesets** are the recommended way to protect
`main`: require pull requests, require status checks, and block force-pushes and deletion.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Workflow shape | one workflow with parallel jobs / one workflow per check | **One `ci.yml`** with parallel jobs: `lint`, `typecheck`, `test` |
| Triggers | push only / PR only / both | **PRs to `main` + pushes to `main`** |
| Action pinning | tags / full SHAs | **Full SHAs** with a version comment; Dependabot keeps them fresh |
| Coverage gate | report only / fail under threshold | **Fail under threshold**, already configured in M1.02 |
| Merge strategy | merge commits / squash / rebase | **Squash merge**: one Conventional Commit per PR, which drives the changelog in M1.07 |

## Tests first

Here the "tests" are tests of the pipeline itself. Prove each one with a throwaway PR:

1. A PR with a **lint error** fails CI and can't be merged.
2. A PR with a **failing test** fails CI and can't be merged.
3. A PR that drops coverage **below the threshold** fails CI.
4. A PR where `pyproject.toml` changed but `uv.lock` wasn't updated fails CI.
5. A clean PR passes and can be squash-merged.

Close the throwaway PRs (or fix them) once you've seen each fail for the right reason.

## Tasks

1. **Create `.github/workflows/ci.yml`** with:
   - triggers on pull requests to `main` and pushes to `main`
   - top-level `permissions` set to read-only contents
   - a `concurrency` group that cancels in-progress runs for the same ref
   - jobs `lint`, `typecheck`, and `test`, each checking out the code, installing uv via the official
     **setup-uv** action with caching enabled, syncing with `--locked`, and calling your runner command
2. **Pin every action to a full SHA** with a trailing comment showing the version.
3. **Upload the coverage report** as a workflow artifact from the `test` job, so you can inspect it.
4. **Protect `main` with a ruleset:** require a PR, require the three CI jobs as status checks,
   require branches to be up to date, block force-pushes and deletion, and allow only squash merges
   (repo settings).
5. **Configure Dependabot** (`.github/dependabot.yml`) for both the **uv** ecosystem and **github-actions**,
   weekly, with grouped minor and patch updates so you're not flooded.
6. **Add a CI status badge** to the README.
7. **Run the five pipeline tests** above.
8. From now on, **all changes go through PRs**, including your own. Get used to it in this module.

## Acceptance criteria

- [ ] `ci.yml` runs lint, typecheck, and test as parallel jobs on PRs and on pushes to `main`
- [ ] Workflow permissions are read-only by default; actions are pinned to full SHAs
- [ ] CI installs with `uv sync --locked` and calls the same runner commands used locally
- [ ] A ruleset on `main` requires the CI checks and a PR, and blocks force-push
- [ ] Dependabot is configured for uv and github-actions
- [ ] The CI badge is visible in the README
- [ ] You've seen a lint failure, a test failure, and a stale lockfile each block a PR

## Stretch

- Add a scheduled weekly run of CI on `main`, so dependency or ecosystem rot shows up even when you're not committing.
- Add **zizmor** or **actionlint** to lint the workflow files themselves.
- Read about the `pull_request_target` trigger and why it's dangerous for public repos.

## References

- GitHub Actions — workflow syntax: https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions
- GitHub Actions — security hardening: https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions
- `GITHUB_TOKEN` permissions: https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication
- uv — GitHub Actions integration: https://docs.astral.sh/uv/guides/integration/github/
- setup-uv action: https://github.com/astral-sh/setup-uv
- Rulesets: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets
- Dependabot configuration: https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference
