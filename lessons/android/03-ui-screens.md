# Android Lesson 03: Core UI screens (Compose + Material 3)

## Outcomes

- Screens match [wireframes](../../design/wireframe-checklist.md) and [product spec](../../design/product-spec.md).
- **Material 3** theming, **Navigation Compose**, and **ViewModel**-driven UI state for each major flow.

## Screens

- Library list
- Book detail
- Add/edit book
- Update progress
- Reading history
- Settings / logout

## Tasks

1. Define **Navigation Compose** routes (typed routes or sealed destinations) for the screens above; keep arguments type-safe where possible (e.g. book id).
2. For each screen, use a **ViewModel** (or official alternative state holder) that exposes **UiState** (loading / success / empty / error) and events.
3. Build **Compose** layouts with **Material 3** components (`Scaffold`, `TopAppBar`, lists, dialogs, text fields). Respect touch targets and accessibility from [UI principles](../../design/ui-principles.md).
4. Wire **repository** calls from ViewModels; use **kotlinx.coroutines** (`viewModelScope`) for async work.
5. Handle **mutations** (create/update/progress): show progress, success snackbar or navigation pop, and refresh list/detail as needed.
6. Validate on **phone and tablet** configurations (window size classes or alternate layouts if you choose).

## Testing

- **Compose UI tests** (`createComposeRule`) for critical flows: e.g. navigation from library to detail, form validation on add book.
- Optional: screenshot or screenshot-diff tooling for regression on key screens.

## Stretch goal: offline-friendly reads

- Cache last-known library payload with **Room** or a serialized snapshot in **DataStore** so the list is readable when offline; show explicit stale/offline state and disable writes or queue them (full offline write queue is post-MVP).

## Acceptance criteria

- Core flows work end-to-end with the API layer from [Lesson 02](02-api-layer.md).
- Loading, empty, and error states are implemented per screen.
- At least one Compose UI test covers navigation or a primary form.
