# M1.07 — First release and rollback

> **Repo:** `books-api` · **Time:** ~3 h · **Prerequisites:** M1.06

## Why

Deploying and releasing are different things. A **deploy** puts a build somewhere. A **release** is a named,
versioned, documented promise: "v0.1.0 is this exact code, here's what changed, here's how to go back."
Consumers (your web and Android apps, later) depend on releases, not on commits.

**Semantic versioning for an API service:** `MAJOR.MINOR.PATCH`, where *breaking changes to the public
contract* bump MAJOR. For `books-api` the contract is the OpenAPI spec. While you're at `0.x`, semver allows
anything to change, so you'll go to `1.0.0` when the API MVP is done (Module 2).

**Conventional Commits → automated changelog.** Since you squash-merge PRs with Conventional Commit titles,
a tool can read history and work out the next version: `fix:` → patch, `feat:` → minor, and `feat!:` or
`BREAKING CHANGE:` → major. It can also write the changelog for you.

**release-please's model:** it keeps an open "release PR" that accumulates the version bump and changelog
as you merge work. **Merging that PR *is* the release**: it creates the tag and the GitHub Release. You
keep a human checkpoint without doing any manual bookkeeping.

**Build once, promote.** The image that passed CI and ran in staging (tagged by SHA) is the one you
release. You add a version tag to the *same image digest* and never rebuild for a release. Rebuilding
means shipping something you didn't test.

**Rollback is a skill you practice before you need it.** Cloud Run keeps previous revisions, so rolling
back is a traffic switch that takes seconds, provided you've done it once while calm.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Release tooling | manual tags / release-please / python-semantic-release | **release-please**: PR-based, human checkpoint, language-agnostic (you'll reuse it for web, Android, and `bookmeta`) |
| Version source of truth | git tags only / `pyproject.toml` version | **`pyproject.toml`**, bumped by release-please and read at runtime via package metadata (M1.03) |
| Tag format | `0.1.0` / `v0.1.0` | **`v0.1.0`** |
| Image promotion | rebuild on tag / re-tag the tested digest | **Re-tag the tested digest** |
| Rollback method | redeploy old image / shift traffic to the previous revision | **Shift traffic** to the previous revision; it's instant and needs no build |

## Tests first

1. Merging a `feat:` PR makes release-please open (or update) a release PR proposing the right version bump
   and changelog entry.
2. Merging the release PR creates the tag `v0.1.0` and a GitHub Release with the generated notes.
3. The image in Artifact Registry tagged `v0.1.0` has the **same digest** as the image tagged with that
   commit's SHA.
4. After the release, staging's `/health` reports version `0.1.0`.
5. **Rollback drill:** after deploying a later change, shifting traffic back to the previous revision makes
   `/health` report the previous SHA. Shifting forward restores the latest.

## Tasks

1. **Configure release-please** for a Python project using the manifest configuration (a config file plus a
   manifest file in the repo). Set the initial version so the first release is `0.1.0`.
2. **Add a `release.yml` workflow** running release-please on pushes to `main`.
   Know this gotcha: events created with the default `GITHUB_TOKEN` **don't trigger other workflows**.
   So do the image re-tagging *inside the same workflow*, as a job that runs only when release-please reports
   a release was created. Don't rely on a separate "on tag" workflow.
3. **Re-tag the image:** in that job, authenticate to GCP via WIF and add the version tag to the existing
   SHA-tagged image in Artifact Registry. Don't build anything.
4. **Write `RELEASING.md`** as a runbook: how a release happens, how to verify it, how to roll back
   (exact steps), and who to notify (you).
5. **Cut `v0.1.0`:** merge the release PR, then verify the tag, the GitHub Release, the image tags, and `/health`.
6. **Run the rollback drill.** Merge a small change (e.g. a docs `fix:`), then use the console or `gcloud`
   to send 100% of traffic back to the previous revision, verify, and restore. Write what you did and
   how long it took into `RELEASING.md`.
7. **Update the README** with a "Releases" section linking the changelog and `RELEASING.md`.

## Acceptance criteria

- [ ] release-please is configured; merging its PR creates a `v`-prefixed tag and a GitHub Release
- [ ] `CHANGELOG.md` is generated from Conventional Commits, with no hand edits needed
- [ ] The release job re-tags the already-tested image digest without rebuilding
- [ ] `v0.1.0` exists, and staging reports version `0.1.0`
- [ ] `RELEASING.md` documents release, verification, and rollback, including the drill's timing
- [ ] The rollback drill was performed and verified with `/health`
- [ ] **Module 1 complete:** run `/lesson-review` for this lesson, then `/session-wrap`

## Stretch

- Add a deploy-time annotation: label the Cloud Run revision with the release version.
- Read about **expand/contract** database migrations now, since they decide whether a rollback is safe once
  a database exists (Module 2).
- Generate a software bill of materials (SBOM) for the image and attach it to the GitHub Release.

## References

- Semantic Versioning: https://semver.org/
- Conventional Commits: https://www.conventionalcommits.org/
- release-please: https://github.com/googleapis/release-please
- release-please-action: https://github.com/googleapis/release-please-action
- GitHub — triggering a workflow from a workflow (`GITHUB_TOKEN` limitation): https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow#triggering-a-workflow-from-a-workflow
- Artifact Registry — tagging images: https://cloud.google.com/artifact-registry/docs/docker/manage-images
- Cloud Run — rollbacks and traffic migration: https://cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration
- Keep a Changelog: https://keepachangelog.com/
