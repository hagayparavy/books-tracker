# Android Lesson 02: API integration layer (Retrofit + Compose app)

## Outcomes

- HTTP client stack aligned with your **OpenAPI** contract ([Shared: OpenAPI workflow](../shared/openapi-workflow.md)).
- Networking isolated from Composables (repositories / use cases consumed by ViewModels).

## Locked choices for this course

- **Retrofit** + **OkHttp**
- JSON with **kotlinx.serialization** (recommended) for request/response models

If you strongly prefer Gson or Moshi, pick **one** serialization stack for the whole app and document it in the Android README.

## Contract alignment

1. Consume the published `openapi.json` from `books-api` (artifact or release URL—no `../books-api` paths).
2. Generate Kotlin models and Retrofit APIs with **OpenAPI Generator**, for example:
   - `kotlin` + Retrofit2 client, or
   - generator templates that emit **kotlinx.serialization**-friendly DTOs.

Regenerate when the API version changes; review diffs in pull requests.

## Tasks

1. Add Retrofit, OkHttp, kotlinx.serialization (or your chosen JSON stack).
2. Configure a base URL from **BuildConfig** or flavor-specific config / `local.properties` overrides—**not** hard-coded for production.
3. Implement an **OkHttp interceptor** that attaches the session token (header or scheme your API defines).
4. Map HTTP errors to **user-safe** errors (e.g. network unavailable vs 401 vs 5xx) for the UI layer.
5. Introduce a **repository** (or use case) layer that screens call; **Composables must not** call `Retrofit` interfaces directly.
6. Cover at least: list books, get book, create/update book, log reading session—per your MVP contract.

## Testing

- Unit-test repository/mappers with **MockWebServer** or mocked Retrofit responses.
- Add at least one test that asserts the auth interceptor adds the expected header when a token is present.

## Acceptance criteria

- API layer matches OpenAPI-generated or manually maintained DTOs without drift from the published spec.
- UI layer depends only on repositories/use cases, not on OkHttp/Retrofit types.
- Regenerating from a new `openapi.json` is a documented, repeatable step.
