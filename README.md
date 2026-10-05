# Rehla Agent Rules Bundle

This bundle provides repository-local rules and reusable skills for agent-driven development.

## Files

- `AGENTS.md` — root operating rules.
- `apps/AGENTS.md` and deeper files — application boundaries.
- `packages/*/AGENTS.md` — package-specific boundaries.
- `.agent/skills/scope-control/SKILL.md` — scope lock and hard stops.
- `.agent/skills/package-boundaries/SKILL.md` — inter-package communication rules.
- `.agent/skills/feature-execution/SKILL.md` — feature execution lifecycle.
- `.agent/skills/convergence/SKILL.md` — spec/plan/tasks-to-code verification.
- `.agent/skills/handoff/SKILL.md` — persistent handoff state.

## Installation

Copy the files into the corresponding paths of the Rehla repository.

## Operating model

`AGENTS.md` provides durable repository guidance; deeper `AGENTS.md` files narrow it for a subtree. Features are executed through `spec.md → plan.md → tasks.md → validation → convergence`.
