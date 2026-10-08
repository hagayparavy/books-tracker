# Product Specification

## Product Vision

Books Tracker helps you maintain a personal reading system across devices:

- Track books to read, reading, paused, completed, dropped.
- Log reading sessions and progress.
- Keep quick notes and ratings for reflection.

## Primary User

- Single power user (you), expanding later to multi-user-ready architecture.

## Core Entities

- User
- Book (title, author, identifiers, cover URL optional)
- Shelf/List (want to read, reading now, completed, custom)
- ReadingSession (date, duration, pages/percentage delta)
- Note (free text, optional tags)
- Rating (optional)

## MVP Scope

- Google sign-in
- Add/edit/delete books
- Move books between statuses/shelves
- Track progress by pages or percentage
- Log reading sessions
- View simple history and current reading summary

## Post-MVP Scope

- Reading goals per month/year
- Streaks and analytics dashboards
- Import/export (CSV/JSON)
- Notifications/reminders

## Success Criteria

- MVP usable on desktop, tablet, mobile browser
- Android app supports same core flows
- All CRUD and progress flows tested
- Release process reproducible from clean machine
