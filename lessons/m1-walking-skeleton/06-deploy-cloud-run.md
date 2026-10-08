# M1.06 — Deploy to Cloud Run

> **Repo:** `books-api` · **Time:** ~4–6 h · **Prerequisites:** M1.05 · **Cost:** ~$0–2/month

## Why

This is the lesson where the skeleton starts walking: every merge to `main` builds an image and deploys
it to a public URL, and no long-lived credential is stored anywhere.

**The GCP pieces and why each exists:**

| Piece | Role |
|---|---|
| **Cloud Run** | Runs your container and scales it, down to zero when idle. Each deploy creates an immutable **revision**. |
| **Artifact Registry** | Private registry for your images, tagged by git SHA. |
| **Secret Manager** | Stores `app_secret`. Cloud Run injects it as an env var at runtime, so it's never in GitHub or the image. |
| **Service accounts** | Identities for machines. You'll use **two**: a *deployer* (used by GitHub Actions) and a *runtime* identity (what the running app is allowed to touch). |
| **Workload Identity Federation (WIF)** | Lets a GitHub Actions run exchange its short-lived **OIDC token** for short-lived GCP credentials. No JSON key files exist, so none can leak. |
| **Budget alert** | Your safety net. It's created first. |

**Keyless auth matters more than it looks.** Downloaded service-account JSON keys are among the most
common causes of cloud breaches in public repos. With WIF, GCP trusts GitHub's identity token *only*
for your specific repository (enforced by an **attribute condition**), and the credentials expire
within an hour.

**Least privilege, concretely:** the runtime account can read *one* secret and nothing else. The deployer
can push images and deploy *one* service, and act as the runtime account, and nothing else. No `Owner`
or `Editor` roles anywhere.

## Decisions you'll make

| Decision | Options | Recommendation |
|---|---|---|
| Region | close to you / cheapest | A **Tier 1 pricing region** reasonably close to you (e.g. `europe-west1`). Check the Cloud Run pricing page. |
| Projects | one project now / staging + production projects now | **One project for staging now.** Production comes in Module 2, when there's a database worth protecting. |
| Infra setup | console clicks / scripted `gcloud` / Terraform | **Scripted `gcloud` commands documented in `docs/infra.md`**: reproducible without a new tool. Terraform is a stretch. |
| Deploy trigger | manual / every push to `main` | **Every push to `main` → staging** |
| Public access | require IAM auth / allow unauthenticated | **Allow unauthenticated**: it's a public API, and app-level auth arrives in Module 2 |
| Image tag | `latest` / git SHA | **Git SHA**, which is immutable and traceable. Never deploy `latest`. |
| GitHub config storage | Actions secrets / environment variables | **Environment variables** on a `staging` GitHub environment. The WIF provider name and project ID aren't secrets. |

## Tests first

Automate these as the final steps of the deploy workflow. A failing check fails the deploy run:

1. `GET <service-url>/health` returns 200 within a short retry window.
2. The reported `git_sha` equals the commit that triggered the deploy. This proves the new revision is
   the one serving traffic.
3. The reported `environment` is `staging`.
4. The app started at all. Because `app_secret` is required, a successful health response proves the
   Secret Manager → Cloud Run → settings path works, without revealing the value.

## Tasks

1. **Budget first.** Create the GCP project, link billing, and create a **budget with alerts**
   (e.g. $5 with 50/90/100% thresholds).
2. **Enable the required APIs:** Cloud Run, Artifact Registry, Secret Manager, IAM Credentials, and Security Token Service.
3. **Create an Artifact Registry** Docker repository in your region, with a **cleanup policy** that keeps
   only the most recent N images.
4. **Create the secret** for `app_secret` in Secret Manager with a strong random value. Never write the
   value into any file in the repo.
5. **Create the runtime service account** and grant it Secret Accessor on **that one secret** only.
6. **Create the deployer service account** with the minimum roles: push to the one Artifact Registry repo,
   deploy Cloud Run services, and *act as* (Service Account User) the runtime account only.
7. **Set up Workload Identity Federation:** a pool, plus a GitHub OIDC provider with an **attribute
   condition restricting it to your repository**. Allow your repo's principal to impersonate the deployer.
8. **Write every command from steps 2–7 into `docs/infra.md`**, so you could recreate the environment
   from scratch. Use placeholder variables for IDs.
9. **Create a `staging` GitHub environment** with variables for the project ID, region, WIF provider,
   and deployer service account email.
10. **Write `.github/workflows/deploy.yml`**, triggered on push to `main` (after CI passes), that:
    - requests the `id-token: write` permission (needed for OIDC) and nothing broader than necessary
    - authenticates with the official Google auth action via WIF
    - builds the image with the git SHA build arg and pushes it tagged with the SHA
    - deploys to Cloud Run with the official deploy action: the runtime service account, the env vars
      (`BOOKS_API_ENVIRONMENT=staging`), and the secret mapped from Secret Manager
    - runs the smoke checks above
11. **Merge a PR and watch it deploy.** Open the URL and `/docs`. Check the Cloud Run revision list.

## Acceptance criteria

- [ ] A budget alert exists and was created before any billable resource
- [ ] No service-account JSON key exists anywhere (check IAM → service account keys: none)
- [ ] WIF provider has an attribute condition restricted to your repository
- [ ] Runtime and deployer service accounts are separate, with least-privilege roles (no Owner/Editor)
- [ ] `app_secret` lives only in Secret Manager and reaches the app as an env var at runtime
- [ ] Every merge to `main` deploys an image tagged with the git SHA to Cloud Run staging
- [ ] The deploy workflow verifies `/health` returns the deployed SHA, and fails otherwise
- [ ] `docs/infra.md` can recreate the whole setup; GitHub config uses environment variables, not secrets
- [ ] Artifact Registry has a cleanup policy

## Stretch

- Recreate the infrastructure with **Terraform** (or OpenTofu) instead of `gcloud` scripts, and compare.
- Make CI and deploy a single pipeline (`deploy` needs `ci`) instead of two workflows. Reason about the trade-offs.
- Set a Cloud Run **max instances** limit as a cost guardrail against traffic spikes.

## References

- Cloud Run — documentation: https://cloud.google.com/run/docs
- Cloud Run — pricing (region tiers, free tier): https://cloud.google.com/run/pricing
- Cloud Run — using secrets: https://cloud.google.com/run/docs/configuring/services/secrets
- Artifact Registry — cleanup policies: https://cloud.google.com/artifact-registry/docs/repositories/cleanup-policy
- Workload Identity Federation with deployment pipelines: https://cloud.google.com/iam/docs/workload-identity-federation-with-deployment-pipelines
- google-github-actions/auth: https://github.com/google-github-actions/auth
- google-github-actions/deploy-cloudrun: https://github.com/google-github-actions/deploy-cloudrun
- GitHub — OIDC in Google Cloud: https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-google-cloud-platform
- Cloud Billing — budgets: https://cloud.google.com/billing/docs/how-to/budgets
