# Course progress

Live state of the course. Updated by `/session-wrap`; read by `/course-status`.
Lesson definitions live in [`lessons/LESSONS-ORDER.md`](lessons/LESSONS-ORDER.md). This file tracks *your* progress.

## Current position

- **Next lesson:** [M1.01 — Repository bootstrap](lessons/m1-walking-skeleton/01-repo-bootstrap.md)
- **Next step:** Decisions made (see below). Install uv + just, then M1.01 task 1 (create the repo).
  Format review happens while working through M1.

## Lesson status

**Status legend:** ⬜ not started · 🟨 in progress · ✅ done

| Lesson | Status | Evidence (PR / commit / tag) |
|---|---|---|
| M0 design: wireframes + Figma | ⬜ | |
| M1.01 Repository bootstrap | ⬜ | |
| M1.02 TDD your first endpoint | ⬜ | |
| M1.03 Configuration and DI | ⬜ | |
| M1.04 CI from day one | ⬜ | |
| M1.05 Container image | ⬜ | |
| M1.06 Deploy to Cloud Run | ⬜ | |
| M1.07 First release and rollback | ⬜ | |

Rows for later modules are added when you start them.

## Decisions

Course-level decisions (also in the ADRs):

- 2026-10-08 — Library track: `bookmeta` (Python, PyPI), consumed by `books-api`. → ADR-0002
- 2026-10-08 — AWS out of scope; to be covered in a separate course. → ADR-0001, README
- 2026-10-08 — Auth stays light: Google third-party sign-in, no deep auth theory.
- 2026-10-08 — Course restructured into modules M0–M6 with a walking skeleton first; CI and releases from M1.
- 2026-10-08 — Rollout: write M1 fully → student reviews format → apply to M2–M6 in one pass, with small touch-ups later.
- 2026-10-08 — Code repos live as subfolders of this workspace, gitignored here, each its own git repo.
- 2026-10-08 — Guidance ladder: hint first; on request, show the concrete solution in chat. → CLAUDE.md rule 1
- 2026-10-08 — Git ownership: student does all git ops in code repos; Claude handles git (commit + push) for this course-docs workspace.
- 2026-10-08 — Git: personal identity (`hagayparavy`, noreply email) and personal SSH key apply automatically
  to this folder and every nested repo via `includeIf` in `~/.gitconfig`. Use **SSH remote URLs** for course repos.

Your implementation decisions (tooling, structure, …) go here with a link to the ADR in your repo:

- 2026-10-08 — `books-api` toolchain (M1.01): **uv** · **src layout** (`src/books_api/`) · **mypy strict** ·
  **Python 3.14** (3.15 just released; wait for wheels, bump later as a practice PR) · **Ruff** defaults +
  `B, UP, I, SIM, N, S` (ignore `S101` assert in `tests/`) · **just** runner · **Conventional Commits** ·
  **MIT**. → to be recorded by the student as `books-api/docs/adr/0001-python-toolchain.md`

## Open questions

- _none_

## Course authoring backlog

- [ ] Student reviews the M1 format and depth → adjust the template and M1
- [ ] Rewrite M2–M6 in the approved format (one pass, possibly in parallel agents), moving legacy drafts into module folders
- [x] Delete absorbed legacy files after the baseline commit
- [x] **Format feedback (M1.01 task 4):** student needed more guidance. Resolved as a **guidance ladder**:
      hint first; if the student asks, show the concrete solution in chat (recorded in CLAUDE.md rule 1).
      For the M2–M6 rewrite: add per-task hints (key names, config shape, doc section) to lessons.
- [ ] Review M1 format while working through it (student hasn't read it all yet). At the end of M1, review
      the session transcripts together to judge how well the lessons taught, then adjust before rewriting M2–M6

## Owed touch-ups

Small edits owed to later lessons, based on decisions made while working:

- M1.01 (and repo-creation steps in M3/M4/M5): say **use the SSH remote URL** up front. GitHub's quick-setup
  page defaults to HTTPS, which routes through `gh` (work account) and fails with 403.
- M1.01 (and M3.01): `git init` before `uv init` means uv **also skips creating `.gitignore`**. The lesson
  should say to write `.gitignore` yourself, or use `uv init --vcs git` semantics knowingly.
- All lessons: weave **git convention hints** into tasks (branch naming, PR-title commit types,
  `--force-with-lease`, never rewrite shared history, tags are permanent). Student wants hints in context,
  **not** a separate git reference doc.

## Session log

- **2026-10-08** — Reviewed the original course plan. Agreed on a restructure: modules M0–M6, a walking
  skeleton first, `bookmeta` library, AWS out of scope. Set up the course memory system (CLAUDE.md,
  PROGRESS.md, skills, lesson-reviewer agent). Wrote Module 1 in the new format.
  **Next:** student reviews M1 → feedback → start M1.01.
