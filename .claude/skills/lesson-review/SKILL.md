---
name: lesson-review
description: Review the student's code for the current (or a named) Books Tracker lesson against its acceptance criteria, using the read-only lesson-reviewer subagent. Use when the student types /lesson-review, asks "review my work", "am I done with this lesson", or "check my repo".
---

# /lesson-review

## Steps

1. **Pick the lesson.** Use the lesson the student named; otherwise the **Current position** in `PROGRESS.md`.
   Read the lesson's header to find its repo (e.g. `books-api`). The repo is the subfolder of that name.
2. **Collect tool output** (this runs in the main session, not the subagent). Look in the repo's README
   or project config for the documented test, lint, and type-check commands, and run them with Bash.
   Read-only commands only: don't install, format, fix, or commit anything.
   Git in the repo is read-only too (`status`, `log`, `diff`): the student does all git operations.
   If commands aren't documented, note that as a finding instead of guessing.
3. **Delegate the review** to the `lesson-reviewer` agent with the Agent tool. Pass: the lesson file path,
   the repo path, and the command output from step 2. The agent is read-only by design; do not do the
   file-by-file review yourself — that is what keeps this session's context small.
4. **Relay the result.** Show the agent's report to the student (it isn't shown to them automatically).
   You may add a short professor's note on top: the single most important thing to fix next.
5. **Ask before updating progress.** If the verdict is **Lesson complete** and the student confirms the
   "needs manual confirmation" items, offer to mark the lesson done in `PROGRESS.md`.

## Rules

- Never write implementation code or edit files inside the repo, even if the student asks for a quick fix.
  Point them at the concept or the doc instead.
