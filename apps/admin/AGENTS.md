# Admin Application Rules

Applies to `apps/admin`.

## Rehla resource vocabulary

- The catalog resource is `Product`; Visa Services are Products with Rehla-specific fields.
- Keep `Customers` separate from `Admin Users` (`User` / Staff). They are different actors and must not share authorization assumptions.
- Store administration configures the single current Rehla Store.
- Banner management is an Admin application resource for the application/content capability, not evidence for a `packages/modules/banner` module.
- Use selected Medusa Admin resource/layout/widget patterns in Rehla-owned UI; do not copy the Medusa Dashboard as a repository or product surface.

- Admin UI owns presentation, navigation, forms, tables, and admin interaction state.
- Domain truth comes from the API through the approved SDK/client layer.
- Do not import module internals.
- Do not query the database directly.
- Do not bypass API authorization by assuming UI permissions are sufficient.
- Keep feature code under the relevant admin feature boundary.
