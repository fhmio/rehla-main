# Shared UI Rules

Applies to `packages/ui/*`.

Rehla's domain decisions are expressed by consuming applications and API data. Shared UI may provide generic resource, form, table, layout, and widget patterns for Products/Visa Services, Customers, Admin Users, Store settings, Applications, and banners, but it must not encode those domain rules or recreate Medusa's Admin product.

- Shared UI contains reusable presentation primitives.
- Do not put domain business rules in shared UI components.
- Do not make the UI package depend on API/domain modules.
- Promote a component here only when it is truly reusable across applications.
