# Scope Control Skill

## Purpose

Keep an implementation agent focused on exactly one approved unit of work.

## Required input

The agent must identify:

- active feature/plan;
- task IDs;
- required dependencies;
- allowed paths;
- non-goals;
- validation command(s).

## Procedure

1. Read applicable `AGENTS.md` files.
2. Read the active `spec.md`, `plan.md`, and `tasks.md`.
3. Inspect only the source files needed to execute the selected tasks.
4. Write down the task boundary before editing.
5. Implement only the selected tasks in dependency order.
6. Run focused validation after each coherent step.
7. Update progress and discoveries.
8. Stop when the assigned tasks and their validation are complete.

## Hard stops

Stop implementation and report the exact conflict when:

- requirements contradict repository rules;
- required dependencies are missing;
- a task requires an architectural decision not present in the plan;
- implementation would require changing an unrelated package;
- the only available solution introduces a dependency cycle.

Do not solve a hard stop by silently expanding scope.

## Out-of-scope discovery

When unrelated work is found:

- record it in `plan.md` under discoveries;
- do not modify it;
- continue only with the assigned scope.
