# API Boundary Rules

Applies to `apps/api`.

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
