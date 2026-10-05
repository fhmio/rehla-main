# Package Boundary Skill

## Purpose

Preserve the Rehla package graph and prevent accidental coupling.

## Decision order for package communication

Use the smallest mechanism that matches the interaction:

1. Local operation inside the owning module.
2. Public module operation for a real, declared dependency.
3. Workflow for a command/process that crosses domain boundaries.
4. Query for a read that spans modules.
5. Link for an association between module-owned records.
6. Event + Subscriber for asynchronous reactions.
7. Provider for an external system.
8. SDK/API for application-to-backend communication.

## Prohibited

- module → another module's private internals;
- module → database tables owned by another module;
- web/admin → database;
- web/admin → module internals;
- provider → domain policy;
- circular module dependencies.

## Required review

Before adding a dependency, confirm:

- the dependency is required by the active feature;
- the target package is the owner of the capability/data;
- the dependency direction does not create a cycle;
- the chosen communication mechanism matches the interaction type.
