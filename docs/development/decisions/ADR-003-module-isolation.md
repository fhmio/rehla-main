# ADR-003 — Domain Module Isolation

## Status
Accepted

## Decision
Each domain module owns its persistence and business rules. Private module internals are not imported by neighboring modules. Cross-module work uses queries, links/associations, and workflows.
