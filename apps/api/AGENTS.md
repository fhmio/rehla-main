# API Boundary Rules

Applies to `apps/api`.

## Rehla domain decisions

- Resolve storefront identity as `Customer` and staff identity as `User`/Admin; keep actor-specific auth and authorization boundaries.
- Resolve Visa catalog operations through the Product capability; do not introduce a separate Visa commerce module.
- Compose checkout into the Rehla `Application` through a workflow from `Cart`; do not add generic Order/shipping/Fulfillment flows.
- Banner is application/content capability. Compose its API access here without moving ownership into `packages/modules/`.
- Rehla has one Store in the current scope. Do not infer multi-store routing or tenant isolation.

## Allowed responsibilities

- HTTP routes/controllers
- input validation at the transport boundary
- authentication/authorization boundary
- workflow composition
- queries
- links
- subscribers
- scheduled jobs
- API configuration/bootstrap

## Forbidden responsibilities

- owning another module's domain model
- direct cross-module database access
- embedding large cross-domain business processes inside routes
- implementing external providers directly inside business logic

## Communication rules

- One-domain operation: call the owning module's public service operation.
- Cross-domain command/process: use a workflow.
- Cross-domain read: use query.
- Cross-domain association: use links.
- Asynchronous reaction: emit/consume events through subscribers.
- External integration: use a provider.
