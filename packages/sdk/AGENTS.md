# SDK Rules

Applies to `packages/sdk/*`.

## Rehla client contracts

- Keep the API vocabulary aligned with `Product` (including Visa Services), `Customer`, separate Admin `User`, the single current Store, `Cart → Application`, and application/content banners.
- Expose banner and domain capabilities only through their approved API contracts; do not leak module internals or imply a Banner module.
- Do not add Order, shipping, or Fulfillment client contracts for the current core.

- SDK is the client-facing contract for applications.
- Keep it transport/client focused.
- Do not place database access or domain implementation in the SDK.
- Do not expose private module internals.
- Add or change SDK contracts only when required by an API capability in the active plan.
