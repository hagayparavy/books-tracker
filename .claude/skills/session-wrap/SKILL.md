---
name: session-wrap
description: End-of-session handoff for the Books Tracker course. Updates PROGRESS.md with lesson status, decisions, open questions, and a session-log entry so the next session can resume cleanly. Use when the student types /session-wrap, says "let's wrap up", "save progress", or is ending a course session.
---

# /session-wrap

Save everything the next session needs into `PROGRESS.md`. Chat history won't survive; this file will.

## Steps

1. Read `PROGRESS.md`.
2. From this session, collect:
   - Lessons whose status changed (not started → in progress → done), with the repo commit or PR link if known.
   - Decisions the student made (tool choices, design choices). Link the ADR in their repo if one exists.
   - New open questions, and touch-ups owed to later lessons.
   - Changes made to course docs this session.
3. Update `PROGRESS.md`:
   - **Current position:** the exact lesson and next step.
   - The lesson status table, **Decisions**, **Open questions**, **Owed touch-ups**.
   - Append a **Session log** entry: date (absolute, YYYY-MM-DD), two or three lines of what happened, next step.
   Keep entries terse. Remove open questions that were resolved.
4. Show the student a short summary of what you recorded.
5. Suggest a commit message for the course workspace if course docs changed. Commit only if the student asks.
