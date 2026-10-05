# Domain Module Rules

Applies to `packages/modules/*`.

## Ownership

Each module owns one domain boundary, including its own models, migrations, and domain service operations.

## Isolation

- Never read another module's tables directly.
- Never import another module's private model/repository/service implementation.
- Do not make a module depend on an application.
- Do not place cross-domain orchestration inside a module.

## Communication

Use the owning public contract plus the appropriate integration mechanism:

- direct public module operation for a genuinely local dependency;
- workflow for multi-domain commands/processes;
- query for multi-domain reads;
- link for cross-domain associations;
- event/subscriber for asynchronous side effects.

## Changes

Do not broaden a module's responsibility to solve an unrelated domain problem.
