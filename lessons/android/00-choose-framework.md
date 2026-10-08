# Android Lesson 00: Framework decision (recorded)

## Decision for this course

**Stack:** Kotlin + **Jetpack Compose** + **Material 3**.

This course assumes native Android UI in Compose, not Flutter or XML-only layouts.

## Why this stack

- Aligns with current Android platform direction and hiring signals.
- Keeps a clear split: **TypeScript** on web, **Python** on the API, **Kotlin** on Android.
- Compose pairs naturally with **Navigation Compose**, **ViewModel**, and modern testing (Compose UI tests).

## Out of scope

**Flutter** is a valid choice for cross-platform UI, but it is **out of scope** for this course track. If you later need iOS or a single UI codebase, revisit with a new ADR.

## Your task

Create **`ADR-0002-android-framework.md`** in the `books-android` repository (use your architecture ADR template) and include:

- **Chosen option:** Kotlin + Jetpack Compose + Material 3
- **Rationale:** bullet list tied to your goals
- **Trade-offs:** e.g. learning curve vs Flutter, platform-specific code
- **First two milestones:** e.g. auth shell + first API-backed list screen

## Acceptance criteria

- ADR exists in `books-android` and matches the locked stack above.
- Team (you) agrees on min SDK / target SDK and package naming before Lesson 01.
