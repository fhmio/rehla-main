# Rehla Foundation and Unified Workspace — Design Specification

**Status:** Draft for user review  
**Scope:** Foundation only  
**Related execution plan:** `docs/development/plans/00-foundation.md` (to be revised after this specification is approved)

## 1. Purpose

Establish a clean, runnable base for Rehla as one independent monorepo. The repository will own its applications, shared packages, developer tooling, and build graph. The existing Rehla UI source will become part of the same root workspace so that applications can consume it locally.

This phase creates working application and package boundaries. It does not implement Rehla business capabilities.

## 2. Current-State Evidence

- The repository has no root `package.json`, lockfile, Turborepo configuration, or CI workflow.
- `apps/api`, `apps/admin`, `apps/web`, `packages/modules`, `packages/providers`, and `packages/sdk` do not yet contain application or package manifests.
- `packages/ui` contains a complete independent Yarn workspace. Its root pins `yarn@3.5.1`, uses `nodeLinker: node-modules`, and contains the `@rehla-ui/ui` component package plus UI tooling and configuration packages.
- The current root `.gitignore` already has Yarn-related exclusions, but the root does not own package installation.
- The current architecture describes `apps/admin` as a React/Vite-like application. The referenced Next.js multi-repository conversation points toward Next.js applications. This is a real decision conflict that must be resolved as part of reviewing this draft.
- The existing architecture and ADR require Rehla to remain independent from Medusa, use explicit API/SDK boundaries, and avoid importing unrelated commerce domains.

## 3. Goals

1. Make the repository a single root-managed workspace with one dependency graph and one lockfile.
2. Integrate the existing `packages/ui` workspaces into that root without losing UI source, behavior, licensing, or package tests.
3. Establish runnable shells for the API, staff Admin, and customer Web applications.
4. Establish shared TypeScript, lint, formatting, test, and build conventions with a root task runner.
5. Enforce the intended dependency direction: applications consume backend capabilities through API contracts/SDK; the API composes modules; shared UI packages contain no Rehla domain behavior.
6. Add a concise developer setup guide and a baseline CI workflow that validates the workspace graph.

## 4. Proposed Decisions for Review

These choices are specific to the foundation draft and remain subject to review:

| Area | Proposed foundation baseline | Rationale |
|---|---|---|
| Repository | One Rehla-owned monorepo; no Medusa repository copy | Matches the accepted ADR and unified workspace decision |
| Package manager | Yarn workspaces, initially retaining the UI repository's Yarn 3.5.1 pin | Reuses the manager already used by the UI source; verify the full dependency graph before locking it |
| Yarn install layout | `nodeLinker: node-modules`; set workspace-level hoisting limits required by the selected Medusa setup | Medusa documents these settings for Yarn monorepos |
| Runtime | Node.js 24 LTS | Shared supported baseline for the current Medusa and Next.js requirements; pin an exact patch through the runtime file during implementation |
| Task runner | Turborepo, with an exact version pin (never `latest`) | Provides dependency-aware root tasks across applications and packages |
| API | Medusa-based Node.js backend application, independently owned by Rehla | Reuses the selected runtime/framework boundary without copying its repository |
| Admin and Web | Next.js App Router applications | Draft follows the Next.js direction implied by the referenced conversation; this conflicts with the current `architecture.md` description of Admin as React/Vite-like |
| Shared UI | Integrate current UI workspaces locally and consume them through workspace dependencies | Implements the user's decision to merge `packages/ui` into one workspace |

Official compatibility references consulted on 2026-10-05:

- [Medusa installation prerequisites](https://docs.medusajs.com/learn/installation)
- [Medusa monorepo and package-manager prerequisites](https://docs.medusajs.com/cloud/projects/prerequisites)
- [Next.js App Router installation requirements](https://nextjs.org/docs/app/getting-started/installation)
- [Next.js 16 upgrade and runtime notes](https://nextjs.org/docs/app/guides/upgrading/version-16)
- [Yarn installation and Corepack](https://yarnpkg.com/getting-started/install)

## 5. Target Repository Shape

```text
rehla/
├── apps/
│   ├── api/                 # Medusa runtime and application composition
│   ├── admin/               # Rehla staff application
│   └── web/                 # Customer-facing storefront
├── packages/
│   ├── contracts/           # Shared API contract/types only
│   ├── modules/             # Domain modules added by later plans
│   ├── providers/           # External adapters added by later plans
│   ├── sdk/                 # Typed client boundary for applications
│   └── ui/                  # Existing Rehla UI source and its workspaces
├── docs/
├── package.json             # Sole workspace manifest
├── yarn.lock                # Sole dependency lockfile
├── .yarnrc.yml
├── turbo.json
└── tsconfig.base.json
```

The exact workspace globs must include the existing UI package, configuration, tooling, and test workspaces. There must be no nested install/lockfile workflow after integration. Existing UI package names and source ownership should be retained unless compatibility evidence requires a reviewed change.

## 6. Runtime Behavior

- A clean checkout can install dependencies from the repository root using the pinned package manager and immutable lockfile.
- One root development command starts the three application shells; each application also has a focused command.
- `apps/api` exposes only a minimal health/readiness response and framework bootstrap. It contains no domain routes or business behavior.
- `apps/admin` and `apps/web` render minimal branded placeholder pages. They contain no auth, catalog, cart, application, payment, or content functionality.
- The UI package can be imported by both frontends through a workspace dependency and can be built/tested through root tasks.
- Root CI installs once and runs affected workspace lint, typecheck, tests, and builds through the task graph. A change in one workspace must not require running unrelated release or deployment jobs.

## 7. Dependency and Ownership Rules

- `apps/admin` and `apps/web` consume backend operations only through `packages/sdk` and shared API contracts; they do not import API internals or module services.
- `apps/api` is the composition root. It may depend on domain modules and approved providers, not the reverse.
- `packages/modules/*` own their own domain persistence and services; no domain modules are implemented in this foundation phase.
- `packages/providers/*` are reserved for external integration adapters; no provider is implemented in this phase.
- `packages/ui` contains generic presentation primitives only. It must not depend on application features or Rehla domain packages.
- No workspace may use a sibling package through a filesystem-relative source path when it should use a declared workspace dependency.
- No dependency cycle is allowed. The workspace task graph must reflect build dependencies, including building `@rehla-ui/ui` before consumers when required.

## 8. Explicit Non-Goals

- Implementing authentication, authorization, Store, Customer, Admin User, or RBAC.
- Implementing Product/Visa Service, pricing, cart, Application, documents, files, payment, tracking, notifications, or banners/content.
- Copying or vendoring the Medusa repository, Medusa Admin, or unrelated Medusa domains.
- Adding a separate API service architecture, microservices, deployment environments, production secrets, or external providers.
- Redesigning the UI component library or changing its public API as part of workspace integration.
- Replacing the established Rehla product/domain decisions in the constitution or ADR.

## 9. Acceptance Criteria

The foundation is acceptable when all of the following are demonstrated by the later implementation plan:

1. A clean checkout can install all workspaces from the root using one lockfile and the pinned runtime/package manager.
2. Root workspace discovery includes each application and each existing UI workspace exactly once.
3. `apps/api`, `apps/admin`, and `apps/web` each start and build independently; the root development task starts the intended applications together.
4. API health/readiness responds successfully; both frontend shells render their placeholder pages.
5. `@rehla-ui/ui` is consumed through a declared workspace dependency by both frontend shells, and its existing focused test/build commands still work from the root.
6. Root lint, typecheck, test, and build tasks complete successfully, with dependency ordering and caching configured without a cycle.
7. CI uses the same root install and verification commands as local development.
8. The README documents runtime setup, installation, focused app commands, root commands, environment variables, and troubleshooting from a clean checkout.
9. Repository review confirms no domain behavior, Medusa repository copy, secrets, or unrelated commerce features entered the foundation.

Automated verification and human review must be recorded separately. The implementation plan must name exact commands and files; no implementation task may be marked complete based only on a successful compilation.

## 10. Decision Required Before the Implementation Plan

Review this draft and settle the Admin framework conflict:

- **Option A (draft proposal):** use Next.js App Router for both `apps/admin` and `apps/web`, then reconcile the architecture documentation to that approved decision in the foundation work.
- **Option B:** keep the Admin as React/Vite-like per the current architecture and use Next.js only for `apps/web`; adjust this specification before planning.

The implementation plan must not be created from this draft until the user approves the specification and this framework choice. Once approved, the plan will translate this design into small, dependency-ordered tasks with exact files, commands, automated checks, and a separate manual review gate.

## 11. Source Documents

- `AGENTS.md`
- `docs/development/constitution.md`
- `docs/development/architecture.md`
- `docs/development/decisions/001-medusa-without-copying.md`
- `docs/development/dependency-graph.md`
- `docs/development/roadmap.md`
- `docs/development/plans/00-foundation.md`
- `packages/ui/package.json`
- `packages/ui/.yarnrc.yml`
