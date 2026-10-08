---
name: course-status
description: Start-of-session briefing for the Books Tracker course. Reads PROGRESS.md and the current lesson, then tells the student where they are, what's next, and what's open. Use when the student types /course-status, says "where are we", "what's next", or starts a new course session.
---

# /course-status

Give the student a short briefing so a fresh session can continue the course without re-reading history.

## Steps

1. Read `PROGRESS.md` at the workspace root.
2. Read the lesson file listed under **Current position**. If none is set, read `lessons/LESSONS-ORDER.md`
   and pick the first lesson that isn't marked done.
3. If the current lesson's repo exists as a subfolder, glance at its `git log --oneline -10` to see what
   changed since the last session log entry. Don't review the code — that is `/lesson-review`'s job.

## Output

Keep it under about 20 lines:

- **Where you are:** module, lesson, step (one line).
- **Last session:** the latest session-log entry, one or two lines.
- **Since then:** new commits in the repo, if any.
- **Next up:** the next one to three concrete tasks from the lesson.
- **Open questions / owed touch-ups:** only the ones relevant to the current lesson.

End by asking whether to continue with the next task or something else. Don't modify any file.
