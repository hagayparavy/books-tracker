# Course progress

Live state of the course. Updated by `/session-wrap`; read by `/course-status`.
Lesson definitions live in [`lessons/LESSONS-ORDER.md`](lessons/LESSONS-ORDER.md). This file tracks *your* progress.

## Current position

- **Next lesson:** [M1.01 — Repository bootstrap](lessons/m1-walking-skeleton/01-repo-bootstrap.md)
- **Next step:** Review the Module 1 format and depth (lessons 01–07), give feedback, then start M1.01 task 1.

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
- 2026-10-08 — Git ownership: student does all git ops in code repos; Claude handles git (commit + push) for this course-docs workspace.
- 2026-10-08 — Git: personal identity (`hagayparavy`, noreply email) and personal SSH key apply automatically
  to this folder and every nested repo via `includeIf` in `~/.gitconfig`. Use **SSH remote URLs** for course repos.

Your implementation decisions (tooling, structure, …) go here with a link to the ADR in your repo:

- _none yet_

## Open questions

- _none_

## Course authoring backlog

- [ ] Student reviews the M1 format and depth → adjust the template and M1
- [ ] Rewrite M2–M6 in the approved format (one pass, possibly in parallel agents), moving legacy drafts into module folders
- [x] Delete absorbed legacy files after the baseline commit
- [ ] Review M1 format while working through it (student hasn't read it all yet). At the end of M1, review
      the session transcripts together to judge how well the lessons taught, then adjust before rewriting M2–M6

## Owed touch-ups

Small edits owed to later lessons, based on decisions made while working:

- _none yet_

## Session log

- **2026-10-08** — Reviewed the original course plan. Agreed on a restructure: modules M0–M6, a walking
  skeleton first, `bookmeta` library, AWS out of scope. Set up the course memory system (CLAUDE.md,
  PROGRESS.md, skills, lesson-reviewer agent). Wrote Module 1 in the new format.
  **Next:** student reviews M1 → feedback → start M1.01.
