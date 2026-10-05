# Feature Execution Skill

## Purpose

Execute one Rehla feature as a small, independently verifiable vertical slice.

## Required artifacts

- `spec.md` — what and why
- `plan.md` — how
- `checklist.md` — requirements quality gate
- `tasks.md` — executable work

## Procedure

1. Confirm the feature is ready and its dependencies are complete.
2. Read the feature artifacts and applicable repository rules.
3. Execute `tasks.md` in dependency order.
4. Keep tests with the feature work rather than postponing them to a project-wide testing phase.
5. Validate each milestone independently.
6. Update `plan.md` with material progress, discoveries, decisions, and outcome.
7. Run convergence before declaring the feature complete.

## Vertical-slice rule

Prefer a complete behavior slice:

`domain → API/workflow → client/UI when required → test → validation`

Do not split a single user-visible capability into disconnected layer-only implementation work unless the active plan explicitly requires it.

## Task completion

A task is complete only when the code change and its relevant verification are complete.
