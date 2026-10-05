# ADR-002 — Runtime Application Boundaries

## Status
Accepted

## Decision
Rehla has three runtime applications: `apps/web`, `apps/admin`, and `apps/api`. Web and Admin consume API contracts; domain packages are not UI runtimes.
