# Web Application Rules

Applies to `apps/web`.

- UI owns presentation, navigation, client interaction, and frontend state.
- Domain truth comes from the API through the approved SDK/client layer.
- Do not import domain module internals.
- Do not query the database directly.
- Do not reimplement backend business rules in client components.
- Keep feature code under the relevant web feature boundary.
