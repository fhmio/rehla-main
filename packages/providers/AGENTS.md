# Provider Rules

Applies to `packages/providers/*`.

Providers are adapters around external systems.

Rehla domain decisions still belong to Rehla modules and workflows: a provider does not create a Visa catalog, Customer/Admin actor model, Store, Application, or Banner domain. Payment, storage, and notification adapters implement only the external contracts required by an approved Rehla capability.

- Keep provider code focused on the external integration contract.
- Do not place Rehla domain workflows or business policy inside a provider.
- Do not allow provider-specific types to leak across the entire domain surface when an internal contract can isolate them.
- Do not add a provider unless an active feature requires that external capability.
