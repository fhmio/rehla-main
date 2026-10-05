# ADR-001 — Monorepo with pnpm and Turborepo

## Status
Accepted

## Decision
Rehla uses one Monorepo with pnpm workspaces and Turborepo as the task graph/orchestration layer.

## Consequence
Applications and domain packages remain versioned together and share one dependency graph.
