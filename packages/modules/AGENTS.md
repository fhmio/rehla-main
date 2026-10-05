# Domain Module Rules

Applies to `packages/modules/*`.

## Rehla domain boundaries

- `Product` owns the catalog primitive; Visa Service is represented as a Product with Rehla-specific fields, not as a separate Visa commerce module.
- `Customer` owns the storefront/business actor. `User` remains the separate Admin/Staff actor and is not folded into Customer.
- The current Store capability represents one Rehla Store; do not add multi-store behavior without an approved decision.
- Do not add a Banner module under this directory. Banner is an application/content capability.
- Do not add generic Order, shipping, or Fulfillment domains to the current service-commerce core.

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
