# UI Principles

## Design Objectives

- Fast entry of reading updates with minimal friction.
- Readable and calm visual language for long-session use.
- Consistent interactions between web and Android.

## Accessibility

- Minimum contrast ratio 4.5:1 for body text.
- Keyboard and screen-reader friendly web interactions.
- Touch targets at least 44x44 px.
- Form validation messages explicit and actionable.

## Responsive Strategy

- Mobile-first layouts.
- Suggested breakpoints:
  - `sm`: 0-639
  - `md`: 640-1023
  - `lg`: 1024+
- Persistent nav on larger screens, compact nav on mobile.

## Navigation Pattern

- Top-level: Library, In Progress, History, Settings.
- Book detail as dedicated route/screen.
- Primary action always visible: Add Book / Log Session.

## Content Patterns

- Book card includes status, current progress, last update date.
- Use progressive disclosure for advanced metadata.
- Empty states should include a clear first action.

## Error and Loading States

- Skeleton loaders for list views.
- Retry affordance for failed network requests.
- Separate offline messaging from server errors.
