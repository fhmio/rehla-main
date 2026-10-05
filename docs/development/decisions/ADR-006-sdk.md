# ADR-006 — Shared SDK Boundary

## Status
Accepted

## Decision
Web and Admin access the API through `@rehla/sdk`. They do not access module persistence or server-only implementation packages.
