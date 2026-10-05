# Provider Rules

Applies to `packages/providers/*`.

Providers are adapters around external systems.

- Keep provider code focused on the external integration contract.
- Do not place Rehla domain workflows or business policy inside a provider.
- Do not allow provider-specific types to leak across the entire domain surface when an internal contract can isolate them.
- Do not add a provider unless an active feature requires that external capability.
