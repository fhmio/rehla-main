# Medusa Compatibility and Selective Reuse Implementation Plan

## Overview
Create the controlled compatibility layer for Medusa framework, selected modules, Admin packages, and design-system packages.

## Current State Analysis
This plan is self-contained. Its Rehla boundaries, contracts, data ownership, and completion criteria are stated below; no source repository or external framework guide is needed.

## Desired End State
Rehla has a production-ready implementation for **Medusa Compatibility and Selective Reuse**, integrated with the previously completed phases and without introducing an upstream Medusa repository copy.

### Key Discoveries
- The current checkout contains no domain module subdirectories under `packages/modules`; verify compatibility against Rehla workspace manifests before adding dependencies or localizing source.
- Medusa modules are independently structured business capabilities with services, models, migrations, types, and tests.
- Medusa application composition is handled separately through API routes, workflows, subscribers, jobs, links, and loaders.
- Medusa Admin separates shell/shared extension infrastructure from resource pages.
- Rehla has one `Store` in the current scope; no multi-store behavior is approved.
- `Product` is the catalog primitive. A Visa Service is a Product with Rehla-specific fields; do not create a separate Visa commerce module.
- `Customer` is the storefront/business actor. `User` is the distinct Staff/Admin actor with a separate authentication context.
- The service-commerce flow is `Cart → Application`; generic Order, shipping, and Fulfillment are outside the current core.
- `Banner` is an application/content capability, not a package under `packages/modules/`. Admin resources and business behavior remain Rehla-owned while adopting selected Medusa patterns.
- Rehla stays an independent repository. Use compatible Medusa packages as dependencies after review; localize only the smallest source unit requiring Rehla-specific customization. Do not add the Medusa repository as a second source tree.
- Use Links for cross-domain associations, Workflows for multi-domain commands, and Events/Jobs for asynchronous side effects.

## What We're NOT Doing
- Copying the Medusa repository or introducing `packages/medusa`.
- Introducing microservices.
- Adding flight booking, hotel booking, or travel-agency marketplace behavior.
- Replacing a Rehla-specific domain with an unrelated Medusa commerce domain merely to reuse code.

## Implementation Approach
Implement in dependency order: owned model → public service contract → API action or workflow → client integration → automated and manual verification. Keep all Rehla domains and routes Rehla-owned.

## Standalone Rehla Contract

This plan is independently executable from this file. Rehla is one repository and one TypeScript/Node modular-monolith API. The API composes runtime, routes, middleware, modules, workflows, subscribers, jobs, Links, and search. Each business module owns its models, migrations, service, validation, and tests under packages/modules/<module>; API application code orchestrates but does not own module persistence. apps/admin and apps/web are Rehla-owned Next.js App Router apps. They use only packages/contracts and packages/sdk to call the API; neither reads the database nor imports module internals. packages/ui contains generic visual primitives only.

The baseline domain is one Store; Customer is the storefront actor; User is staff; Product is a Visa Service; Cart contains selected services; Application is the operational request. The canonical path is Cart → Application → Documents → Payment → Tracking. Do not add generic Order, shipping, fulfillment, physical inventory, flights, hotels, agency marketplace, or a Banner module. Banner is API application content. Use public module contracts for synchronous calls, Links for cross-module relationships, Workflows for multi-module commands, and Events/Jobs for asynchronous side effects. PostgreSQL is transactional truth, Redis is infrastructure, and sensitive files are private behind File. Server-side validation, ownership, authorization, price, and state transitions are authoritative.

## Plan-Specific Rehla Contract

### Compatibility baseline
The supplied source snapshot identifies the framework/runtime at version 2.21.2. Rehla consumes pinned packages as dependencies only; it does not vendor the source tree, import private package paths, copy Admin Dashboard code, or allow upstream defaults to expand Rehla scope. Package adoption is conditional on public exports, license, peer/runtime requirements, provider support, dependency graph, and API compatibility review.

| Decision | Rehla treatment |
|---|---|
| Framework/runtime, module SDK, workflows SDK, query, HTTP/API composition | Use pinned public contracts in apps/api; Rehla owns configuration, routes, modules, and business behavior. |
| Store, Customer, User, Auth, Product, Pricing, Currency, Region, Cart, Payment, File, Notification, Translation, Search, Settings, Analytics, API Key | Adapt only the entities and service behavior needed by the Rehla contracts. |
| Links, event bus, workflow engine, jobs, locking, caching | Use as infrastructure boundaries; publish only Rehla-owned link/event/workflow contracts. |
| Application, Documents, Tracking, Bank Transfer provider, Banner/content | Implement as Rehla-owned capabilities with their own schemas and lifecycles. |
| Admin and Web surfaces | Implement as Rehla Next.js apps; use interaction and contract patterns only. |
| Fulfillment, Inventory, Stock Location, Order, shipping, marketplace/vendor, Promotion, Tax, physical logistics | Exclude unless separately approved by a project decision. |

### Adoption gates
For each dependency, record package name/version, public entry point, license, peer/runtime requirements, required behavior, Rehla adapter, and prohibited transitive behavior. Keep compatible runtime packages version-aligned at 2.21.2. Reject Enterprise-only or otherwise incompatible code unless its use and license are explicitly cleared. Add a contract test for each adopted public service/provider. Localization is permitted only for an explicitly owned Rehla behavior that cannot be expressed through a public contract, with a reason, maintained diff, tests, and compatibility note. Completion requires a checked matrix and proof that no excluded module is activated.

## Phase 1: Dependency/version baseline

### Changes Required
- Deliver only the schema, migration (when persistent state changes), public service contract, API/client interface, and named tests required by the Plan-Specific Rehla Contract in this file.
- Execute and persist behavior only in the owner named in this plan; expose public contracts and do not import another module internals or move domain behavior into a UI.
- Add versioned migrations only for owned persistent data; define the public request/response contract and required tests before exposing the capability to another surface.

### Success Criteria

#### Automated Verification
- [ ] Typecheck passes for all affected workspaces.
- [ ] Lint passes with no new violations.
- [ ] Relevant unit/integration tests pass.
- [ ] Relevant application build succeeds.
- [ ] Regression tests for dependency boundaries pass.

#### Manual Verification
- [ ] Verify the phase behavior in the relevant Admin/Web/API flow.
- [ ] Verify error/empty/loading states and the important edge cases.
- [ ] Verify no excluded Rehla scope has been introduced.

**Implementation Note:** Complete automated verification before moving to the next phase. Manual verification remains unchecked until a human confirms it.

## Phase 2: Custom module contract

### Changes Required
- Implement custom module contract as specified in the Plan-Specific Rehla Contract above, including ownership, inputs/outputs, server-side validation/authorization, failure handling, and acceptance cases.
- Publish only the operations listed in this plan, with their actor, ownership, input/output, error, pagination, and idempotency contract.
- Implement the negative, permission, failure, retry, and idempotency cases listed in this plan; state explicitly when an operation is not retryable.

### Success Criteria

#### Automated Verification
- [ ] Typecheck passes for all affected workspaces.
- [ ] Lint passes with no new violations.
- [ ] Relevant unit/integration tests pass.
- [ ] Relevant application build succeeds.
- [ ] Regression tests for dependency boundaries pass.

#### Manual Verification
- [ ] Verify the phase behavior in the relevant Admin/Web/API flow.
- [ ] Verify error/empty/loading states and the important edge cases.
- [ ] Verify no excluded Rehla scope has been introduced.

**Implementation Note:** Complete automated verification before moving to the next phase. Manual verification remains unchecked until a human confirms it.

## Phase 3: Selective localization and provenance rules

### Changes Required
- For selective localization and provenance rules, use the exact public contract, Link, Workflow, Event, or Job assigned to each operation in this plan; do not use private cross-module access.
- Connect the named surface through packages/sdk and packages/contracts after the API/action contract is tested; keep authoritative validation and business rules on the server.
- Record rollback/compensation, data-retention behavior, and operator-visible failure signals for each persistent or irreversible change.

### Success Criteria

#### Automated Verification
- [ ] Typecheck passes for all affected workspaces.
- [ ] Lint passes with no new violations.
- [ ] Relevant unit/integration tests pass.
- [ ] Relevant application build succeeds.
- [ ] Regression tests for dependency boundaries pass.

#### Manual Verification
- [ ] Verify the phase behavior in the relevant Admin/Web/API flow.
- [ ] Verify error/empty/loading states and the important edge cases.
- [ ] Verify no excluded Rehla scope has been introduced.

**Implementation Note:** Complete automated verification before moving to the next phase. Manual verification remains unchecked until a human confirms it.

## Testing Strategy
- Unit tests for models, services, validators, and pure logic.
- Integration tests for database behavior, module links, providers, and workflows.
- HTTP tests for auth, permissions, validation, and response contracts.
- Admin/Web component and critical-path E2E tests where the phase changes UI behavior.

## Performance Considerations
Measure database query count, payload size, API latency, cache behavior, and background-job impact for the phase. Do not optimize by adding infrastructure that the phase does not require.

## Migration Notes
Use clean Rehla migrations for new domains. Do not replay unrelated historical Medusa migrations. Any localized Medusa module migration must be reconciled to the current Rehla schema before deployment.

## Self-containment
This plan includes its Rehla-specific scope, interfaces, ownership, security rules, ordered deliverables, and acceptance criteria. Internal plans may be used for sequencing only; implementation does not require access to a Medusa repository or external source files.