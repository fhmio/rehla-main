# Admin Application Rules

Applies to `apps/admin`.

- Admin UI owns presentation, navigation, forms, tables, and admin interaction state.
- Domain truth comes from the API through the approved SDK/client layer.
- Do not import module internals.
- Do not query the database directly.
- Do not bypass API authorization by assuming UI permissions are sufficient.
- Keep feature code under the relevant admin feature boundary.
