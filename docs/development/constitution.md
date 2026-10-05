# Rehla Development Constitution

## Status

ACTIVE — 2026-10-03

## Purpose

This document contains project-wide rules that all Rehla implementation plans and code changes must follow.

## 1. Architecture

- Rehla is a single Monorepo.
- Workspace management uses pnpm.
- Turborepo is the task-graph/orchestration layer.
- Runtime applications are `apps/web`, `apps/admin`, and `apps/api`.
- Domain capabilities are isolated workspace packages under `packages/modules/`.
- External integrations are isolated under `packages/providers/`.
- Shared client access is through `packages/sdk/client`.
- Shared UI and repository configuration remain separate from business domains.
- Rehla is a modular monolith; no microservices boundary is introduced by a feature plan.

## 2. Module Isolation

- A domain module owns its models and business rules.
- A module must not import another module's private implementation or persistence objects.
- Cross-domain reads use the approved query/read mechanism.
- Cross-domain relationships use explicit links/associations.
- Multi-domain business orchestration belongs in API-level workflows.

## 3. API Boundary

- `apps/api` is the only backend runtime boundary exposed to clients.
- Web and Admin consume the API through the shared SDK.
- HTTP contracts are defined and checked through OpenAPI.
- Routes perform transport/auth/validation work and delegate business orchestration.
- Business behavior must not be duplicated between Web, Admin, and API clients.

## 4. External Integrations

- Provider packages isolate storage, payment, and notification integrations.
- Provider-specific details must not leak into domain models unless represented as explicit provider metadata.
- Adding a provider does not change the domain contract unless a documented domain capability requires it.

## 5. Feature Scope

- Every feature plan represents one coherent capability with an independently verifiable outcome.
- Plans use vertical slices when a user-visible capability crosses domain/API/UI boundaries.
- Non-goals must be explicit.
- Unrelated refactors are out of scope unless required to satisfy the feature's acceptance criteria.

## 6. Rehla Product Scope

In scope:

- Visas.
- Tourist visits.
- Umrah services.
- Applications.
- Documents.
- Tracking.
- Customer wallet.
- Wallet funding.
- Bravo, CashiPay, and Bank Transfer funding paths.
- Manual bank-transfer review.
- Banners.
- Notifications.
- Admin operations.
- Reporting.

Out of scope:

- Flight booking.
- Hotel booking.
- Marketplace / agency offers.

The current requirements source explicitly states these boundaries. See the source-alignment note in `architecture.md`.

## 7. Payment Scope Boundary

The current payment requirements define wallet funding in detail. They do not establish wallet balance as the payment method for visa/service applications. Plans must not implement that linkage unless a later approved decision explicitly adds it.

## 8. Data and Security

- Customer data is owner-scoped.
- Admin operations require explicit authorization.
- Sensitive uploads are private storage objects with controlled access.
- Passwords, tokens, passport numbers, and document contents must not be logged.
- Money is represented with integer minor units plus ISO currency code; floating-point arithmetic is not used for monetary values.
- Accepted status transitions create immutable tracking history.

## 9. Testing and Validation

- A feature is not complete without automated tests appropriate to its risk.
- Unit tests validate local business rules.
- Integration tests validate package/database/provider boundaries.
- End-to-end tests validate critical customer-to-admin workflows.
- Validation must prove behavior, not only compilation.

## 10. Agent Execution

Agents must:

- Read the relevant domain README, feature `spec.md`, `plan.md`, and `tasks.md` before implementation.
- Respect declared dependencies and parallelization markers.
- Keep changes inside the feature boundary.
- Update `Progress`, `Discoveries`, `Decision Log`, and `Outcomes` in `plan.md` during execution.
- Never silently change the scope. Scope changes require a documented decision and updated artifacts.
- Run the validation defined in the plan before marking the feature complete.

## 11. Documentation

- `constitution.md` contains durable project rules.
- `architecture.md` describes the current system shape.
- `roadmap.md` describes execution order and dependency flow.
- `decisions/` records cross-feature architectural decisions.
- Each domain has a `README.md` that indexes its features.
- Each feature uses the same bundle: `spec.md`, `checklist.md`, `plan.md`, `tasks.md`.
