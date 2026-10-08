# Android Lesson 01: Project setup and auth (Kotlin + Compose)

## Outcomes

- Android Studio project with **Jetpack Compose** and **Material 3**.
- **Navigation Compose** shell (logged-out vs logged-in destinations).
- Google sign-in integrated with your backend; tokens stored safely.

## Prerequisites

- `ADR-0002-android-framework.md` in `books-android` matches Kotlin + Compose + Material 3.
- Backend auth endpoint from [Backend Lesson 03](../backend/03-auth-google.md) is defined or stubbed with a contract.

## Tasks

1. Create a new project: **Empty Compose Activity** (or equivalent wizard), Kotlin, Gradle with Kotlin DSL.
2. Enable **Navigation Compose**: a start destination for sign-in and a nested graph or routes for the main app after auth.
3. Integrate Google sign-in using current Google guidance:
   - **Credential Manager** (preferred for new work), and/or
   - **Google Sign-In for Android** (`com.google.android.gms:play-services-auth`) where still appropriate for your flow.
   Document which API you used in the repo README.
4. After the user signs in with Google, **exchange** the Google ID token (or auth code, per your backend design) with your **`books-api`** endpoint; persist only what the backend requires (e.g. short-lived access token, refresh handling per your API).
5. Store session material with **Jetpack DataStore** (Preferences) for non-sensitive flags and **EncryptedSharedPreferences** (AndroidX Security crypto) or another approved pattern for secrets/tokens—**never** store the OAuth **web client secret** on the device (mobile clients use the appropriate Android OAuth client ID; secrets stay server-side).
6. On app start, resolve auth state: if valid session → main graph; else → sign-in.
7. Implement **logout**: clear tokens and navigate to sign-in.

## Security requirements

- No API keys or client secrets committed to git.
- Do not log raw tokens.
- Use **network security config** and HTTPS only for API calls.

## Acceptance criteria

- Login and logout work end-to-end against your backend (or staging).
- Unauthenticated users cannot reach main app routes (guard in Navigation or ViewModel layer).
- Token storage follows Android security guidance above.

## Suggested reading order in codebase

`Application` / entry → NavHost → Sign-in screen (Compose) → Auth repository → API auth call.
