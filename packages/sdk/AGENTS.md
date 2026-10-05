# SDK Rules

Applies to `packages/sdk/*`.

- SDK is the client-facing contract for applications.
- Keep it transport/client focused.
- Do not place database access or domain implementation in the SDK.
- Do not expose private module internals.
- Add or change SDK contracts only when required by an API capability in the active plan.
