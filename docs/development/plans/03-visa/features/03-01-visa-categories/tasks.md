# Visa Service Categories — Tasks

## Phase 1 — Specification and Preparation

- [ ] T001 Confirm `spec.md` is clarified and `checklist.md` has no unresolved requirement-quality failure.
- [ ] T002 Verify dependency completion: - None
- [ ] T003 Inspect `packages/modules/visa + apps/api` and record the exact existing files/contracts that this feature will modify.

## Phase 2 — Foundational Domain Work

- [ ] T004 Implement the owning capability for **Visa Service Categories** inside the declared boundary.
- [ ] T005 Add focused unit tests for the feature's primary business rules and failure cases.

## Phase 3 — Integration

- [ ] T006 Add only the required Workflow / Query / Link / Provider integration for **Visa Service Categories**.
- [ ] T007 Update the API surface and authoritative OpenAPI contract when **Visa Service Categories** requires an HTTP capability.
- [ ] T008 Add integration coverage for the affected package boundaries.

## Phase 4 — Client Integration

- [ ] T009 Update `apps/web` only when **Visa Service Categories** has a customer Web surface.
- [ ] T010 Update `apps/admin` only when **Visa Service Categories** has an operational Admin surface.
- [ ] T011 Add focused UI/E2E coverage for the acceptance scenarios that cross the runtime boundary.

## Phase 5 — Validation and Convergence

- [ ] T012 Run targeted tests, typecheck, and build for affected workspaces.
- [ ] T013 Verify authorization, ownership, invalid state, retry/idempotency, and provider-failure cases applicable to **Visa Service Categories**.
- [ ] T014 Update `plan.md` with actual progress, discoveries, decisions, changed boundaries, and validation evidence.
- [ ] T015 Run convergence against `spec.md`, `plan.md`, and `tasks.md`; append any missing work as new tasks.

## Dependencies

- None

## Parallel Execution

Use `[P]` only after checking the actual repository dependency graph and file ownership. Two tasks are parallel only when neither consumes the unfinished output of the other and both have independent validation.

## Implementation Strategy

Execute the smallest complete vertical slice for **Visa Service Categories**, validate it, then continue to the next phase. Do not broaden the feature to adjacent capabilities merely because their code is nearby.
