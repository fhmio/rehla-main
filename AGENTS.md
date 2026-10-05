# Rehla Agent Operating Rules

## 1. Mission

Rehla is a single monorepo. Work is executed incrementally through approved plans and tasks.
The agent's job is to complete the current scoped task, preserve the existing architecture, and leave a verifiable result.

## 2. Source of truth

Read only the minimum context needed for the current task, in this order:

1. This `AGENTS.md` and any deeper applicable `AGENTS.md` files.
2. `docs/development/constitution.md`
3. `docs/development/architecture.md`
4. The relevant stage/domain `index.md`.
5. The target feature's `spec.md`, `plan.md`, `checklist.md`, and `tasks.md`.
6. Only the relevant source files, tests, and references.

Do not load the entire repository unless the task explicitly requires repository-wide analysis.

## 3. Scope lock

Before editing:

- Identify exactly one active stage and one active feature/plan unless the plan explicitly names more.
- State the task IDs being executed.
- Identify the allowed files/directories and direct dependencies.
- Identify explicit non-goals.

During editing:

- Implement only the approved scope.
- Do not refactor unrelated code.
- Do not rename, move, or delete unrelated files.
- Do not add a new package, application, framework, dependency, provider, abstraction, or architectural pattern unless the active plan explicitly requires it.
- Do not change product requirements, architecture, or global decisions inside an implementation task.
- Do not silently expand scope because a nearby improvement is noticed.

When unrelated work is discovered:

- Record it as a discovery.
- Do not implement it unless the active plan is updated through the project's planning process.

## 4. No assumption drift

Use repository evidence and the active specification as the source of intent.

- Do not invent missing requirements.
- Do not reinterpret an explicit requirement for convenience.
- Do not replace an existing project decision with a personal preference.
- When the current artifacts conflict, stop implementation of the conflicting part and report the exact contradiction with file references.

## 5. Dependency discipline

Respect the Rehla package graph.

- Applications consume backend capabilities through the approved API/SDK boundary.
- Domain modules own their own data and domain services.
- Do not read another module's database tables, repositories, internal services, or private models directly.
- Use Workflows for cross-domain business orchestration.
- Use Query for cross-module reads.
- Use Links for cross-module associations.
- Use Events/Subscribers for asynchronous side effects.
- Use Providers only for external integrations.
- Do not create dependency cycles.

## 6. Change discipline

Prefer the smallest coherent change that satisfies the active task.

A change is out of scope when it:

- serves a different feature;
- changes an unrelated public API;
- changes a different module's internal design;
- changes architecture without an approved decision;
- changes formatting broadly without need;
- fixes unrelated technical debt.

## 7. Plan execution

`tasks.md` is the executable work list.

- Execute tasks in dependency order.
- Respect `[P]` parallel markers only when their dependencies and file ownership are independent.
- Do not mark a task complete until its implementation and validation are complete.
- Keep `plan.md` updated with material progress, discoveries, decisions, and outcomes.

## 8. Verification

Every implementation must be verified at the narrowest useful level and then at the feature level.

At minimum, run the checks required by the active plan. Prefer:

1. focused unit/module tests;
2. focused integration/API tests;
3. typecheck/lint/build when affected;
4. feature acceptance validation.

Never claim completion from compilation alone when behavior can be tested.

## 9. Convergence

Before declaring a feature complete, compare the implementation against:

- `spec.md`
- `plan.md`
- `tasks.md`
- `constitution.md`

Check for missing, partial, contradictory, and unrequested work.
If a gap is found, record it as a convergence task and return to implementation.
Do not hide missing work by changing the specification to match an incomplete implementation.

## 10. Handoff

Leave enough state for another agent to continue without conversation history.
Update the feature `plan.md` with:

- current progress;
- material discoveries;
- decisions made and why;
- remaining work;
- validation performed;
- final outcome.

## 11. Protected areas

Do not modify the following during a feature task unless explicitly included in the active plan:

- project constitution;
- global architecture decisions;
- roadmap;
- unrelated plans;
- unrelated modules/packages/apps;
- CI/deployment/security configuration unrelated to the task.

## 12. Completion rule

The task is complete only when:

- all assigned task items are implemented;
- relevant tests pass;
- required validation passes;
- no unauthorized scope was added;
- package dependency rules remain valid;
- progress/outcome is recorded;
- the feature can pass convergence.

## 13. Working style

Be decisive about the task boundary, conservative about changes, explicit about dependencies, and evidence-driven about repository behavior.
