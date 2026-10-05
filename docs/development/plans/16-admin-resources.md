# Rehla Admin Resources Implementation Plan

## Overview
Build Store, Product/Visa Service, Customer, Admin User, Application, Document, Payment, Tracking, Banner, Settings, and Reporting resources.

## Current State Analysis
The supplied Medusa source is a large modular monorepo separating reusable modules, framework/core, Admin, design system, application composition, plugins, and integration tests. Rehla adopts the relevant boundaries and patterns but remains a separate project. The supplied create/implement plan rules require full context, ordered phases, measurable automated/manual verification, and no unchecked open questions.

## Desired End State
Rehla has a production-ready implementation for **Rehla Admin Resources**, integrated with the previously completed phases and without introducing an upstream Medusa repository copy.

### Key Discoveries
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
Implement from the domain boundary outward: schema/model → service → workflow/event/link → API → Admin/Web integration → verification. Keep Rehla independent. After compatibility review, consume compatible Medusa packages as dependencies and localize only the smallest source unit requiring Rehla-specific customization; never add the Medusa repository as a second source tree.

## Phase 1: Commerce/admin resources

### Changes Required
- Create the files/modules/configuration needed for commerce/admin resources.
- Keep ownership inside Rehla and preserve the documented module/application boundary.
- Add migrations/contracts/tests before exposing the feature to downstream phases.

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

## Phase 2: Operations resources

### Changes Required
- Implement operations resources using the established Rehla/Medusa-compatible pattern.
- Expose only the endpoints and APIs required by the established scope.
- Add negative/error-path tests and idempotency where mutations cross boundaries.

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

## Phase 3: Content/settings/reporting resources

### Changes Required
- Integrate content/settings/reporting resources with dependent modules through Links, Events, or Workflows.
- Add Admin/Web integration only after the backend contract is verified.
- Document operational and rollback behavior for the phase.

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

## References

- `docs/development/constitution.md` — confirmed Rehla scope and operating rules.
- `docs/development/decisions/001-medusa-without-copying.md` — Medusa reuse and domain decisions.
- `docs/development/dependency-graph.md` — plan ordering and dependencies.

