# Web Application Rules

Applies to `apps/web`.

## Rehla storefront decisions

- The end customer is a `Customer`; Admin/Staff `User` accounts do not authenticate through the customer storefront context.
- Present Visa services as catalog `Products`, preserving the Rehla service-specific fields supplied by the API.
- The customer journey uses `Cart → Application` and has no shipping/Fulfillment journey.
- Banners are application/content presented by the storefront through the API; the Web app does not own banner persistence or business rules.
- The current storefront is for one Rehla Store; do not assume multi-store selection.

- UI owns presentation, navigation, client interaction, and frontend state.
- Domain truth comes from the API through the approved SDK/client layer.
- Do not import domain module internals.
- Do not query the database directly.
- Do not reimplement backend business rules in client components.
- Keep feature code under the relevant web feature boundary.
