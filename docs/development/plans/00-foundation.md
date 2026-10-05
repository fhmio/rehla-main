# Rehla Foundation and Unified Workspace Implementation Plan

> **For agentic workers:** Implement this plan task-by-task with the Superpowers executing-plans skill. Treat each numbered task as a review gate. Do not begin implementation until the plan itself is approved.

**Goal:** Build a runnable Rehla monorepo foundation with one Yarn workspace, the integrated Rehla UI, an independently owned Medusa API, and Next.js App Router applications for Admin and Web.

**Architecture:** Rehla is one monorepo and remains independent from the Medusa repository. Yarn owns one workspace graph and lockfile; Turborepo orders package and application tasks. The Medusa API exposes a minimal health contract, Next.js Admin and Web consume shared UI and API contracts through the SDK, and no business domains are implemented.

**Tech Stack:** Node.js 24 LTS, Yarn 3.5.1 workspaces with node-modules linker, Turborepo 2.11.6, Medusa 2.21.2, Next.js 16.2 App Router with React 19.2, TypeScript, Prettier 3.9.6, PostgreSQL 17 for local development, GitHub Actions.

**Decision record:** Option A is approved: both Admin and Web use Next.js App Router. The implementation requirements and acceptance gates are also included in this plan.

**Plan Status:** Ready for user review; implementation not started.

## Planning Scope

- Active stage: 00 Foundation and Unified Workspace.
- Planned task IDs: FND-01 through FND-07, listed as Tasks 1–7 below.
- This artifact defines implementation only; do not begin code changes until this plan is approved.
- Explicit non-goals: domain features, credentials/providers, production deployment, and changes to the constitution or product/domain decisions.

## Standalone Rehla Contract

This foundation creates the executable repository/runtime boundary used by every later plan. Rehla is one independent Yarn workspace and a TypeScript/Node modular monolith. `apps/api` composes the Medusa 2.21.2 runtime and owns HTTP routes, middleware, startup composition, workflows, subscribers, jobs, links, and search indexing. Domain state belongs to the owning package under `packages/modules/*`; application code does not own module tables. `apps/admin` and `apps/web` are separate Rehla-owned Next.js App Router applications. Both call `apps/api` through `packages/sdk` and `packages/contracts`; neither imports API/module internals or accesses the database. `packages/ui` is generic design-system code only.

The business vocabulary established for the workspace is: one `Store`; `Customer` for the storefront actor; `User` for staff; `Product` for the sellable Visa Service; `Cart` for selected services; and `Application` for the operational visa request. The process is `Cart → Application → Documents → Payment → Tracking`; there is no generic Order, shipping, fulfillment, flight, hotel, marketplace, agency, or physical-inventory domain. Banner/content is an API application feature, not a module package. Links represent cross-module associations, Workflows coordinate multi-module operations, and Events/Jobs perform asynchronous reactions. Backend validation, authorization, pricing, and state transitions are authoritative. PostgreSQL is transactional system of record; Redis supports cache/locks/runtime; sensitive files use a private File abstraction/provider.

This plan defines the workspace, app, API health contract, and test interfaces below. Later plans must repeat the Rehla-specific contracts they need and must not require opening the Medusa source repository or external framework documentation to understand their tasks.

## Global Constraints

- Use Node.js 24 LTS and record the major in .node-version.
- Pin the root package manager to Yarn 3.5.1 with Corepack and commit one root yarn.lock.
- Configure Yarn with nodeLinker: node-modules and nmHoistingLimits: workspaces.
- Pin Turborepo to 2.11.6; do not use latest.
- Use Medusa 2.21.2 and align every @medusajs/* dependency that follows the Medusa 2.x version line to 2.21.2.
- Use Next.js 16.2 with App Router and React/React DOM 19.2 in both apps/admin and apps/web; validate the shared UI package against React 19 before integration.
- Keep Rehla independent; never copy or vendor the Medusa repository, Medusa Admin product, or unrelated commerce domains.
- Keep app-to-backend dependencies behind packages/contracts and packages/sdk; do not import API or module internals into frontend apps.
- Preserve all existing UI package source, license, and component API; limit UI package changes to workspace/tool integration and required React 19.2 compatibility.
- Do not make apps depend on a root tsconfig file: Medusa Turborepo pruning does not include arbitrary root files in the pruned build.
- Keep local PostgreSQL development configuration separate from production/deployment configuration.
- Keep domain features, auth, payments, banners, providers, deployment, and production secrets outside this foundation.

## Current-State Discoveries and Approved Decisions

- The user approved Next.js App Router for both Admin and Web on 2026-10-05. The existing architecture's React/Vite-like Admin entry must be updated before implementation.
- The existing UI workspace has six leaf workspaces plus its private root package, a Yarn 3.5.1 release, two Yarn plugins, and a class-variance-authority patch. Migrate these assets and local resolutions before deleting nested package-manager files.
- The shared UI package currently declares React 18 peer/dev versions, while Next.js 16's App Router uses React 19.2. Keep the component API stable, validate the library under React 19.2, and expand its peer range to include the tested React 19 line before consuming it from either app.
- Medusa API route tests use `@medusajs/test-utils` and a real PostgreSQL test database. Local Compose and CI PostgreSQL service are part of the runnable API foundation, not production infrastructure.
- Do not make the Medusa build depend on a root TypeScript config; Medusa's Turbo-pruned build omits arbitrary root files.

## Review Focus

- Missing or duplicate root workspace discovery, especially nested packages/ui/configs, packages/ui/packages, packages/ui/tests, and packages/ui/tools — Task 2 runs scripts/verify-workspaces.mjs against the complete expected workspace set.
- UI Yarn patch/plugin paths or resolution changes after merging the nested project — Task 2 runs yarn install --immutable and the UI package build/test commands.
- React peer or CSS export incompatibility when both Next.js apps consume the UI library — Task 5 runs each Next build and renders both pages locally.
- Medusa build failure caused by a shared root file omitted by Turborepo pruning — Task 4 builds the API and Task 7 runs turbo prune for @rehla/api.
- Workspace task cycles or missing dependency ordering — Task 3 runs Turborepo dry-run graph checks before the full build.

## Foundation Acceptance Criteria

The foundation is complete only when all criteria below have recorded evidence:

1. A clean checkout installs all workspaces from the repository root with one lockfile and the pinned runtime/package manager.
2. Root workspace discovery includes each app and every existing UI workspace exactly once.
3. API, Admin, and Web each start and build independently; the root development command starts the intended applications together.
4. API health/readiness succeeds and both frontend shells render their placeholder pages.
5. Both frontend shells consume `@rehla-ui/ui` through declared workspace dependencies, and existing UI test/build commands work from the root.
6. Existing UI tests/build pass with React 19 while public exports and component behavior remain stable.
7. Root lint, typecheck, test, and build pass with correct dependency ordering and no task cycle.
8. CI uses the same root install and verification commands as local development.
9. README provides clean-checkout runtime setup, install, focused app commands, root commands, environment variables, and troubleshooting.
10. Review confirms no business-domain implementation, repository/source copy, secrets, or excluded commerce behavior entered the foundation.

Automated evidence and human review are recorded separately. A build alone cannot close behavior, security, or manual-review criteria.

---

### FND-01: Reconcile the approved framework decision in project guidance

**Files:**
- Modify: docs/development/architecture.md
- Modify: docs/development/decisions/REHLA_DECISIONS.md
- Modify: README.md
- Modify: docs/development/plans/README.md
- Modify: AGENTS.md
- Modify: apps/AGENTS.md
- Modify: apps/admin/AGENTS.md
- Modify: apps/web/AGENTS.md

**Interfaces:**
- Consumes: The approved decision in the spec: Next.js App Router for both Admin and Web.
- Produces: Consistent written guidance that names the API as the Medusa runtime, Admin and Web as Rehla-owned Next.js applications, and the existing plan/spec relationship.

- [ ] Use the approved Option A decision recorded in this plan: Next.js App Router for both Admin and Web.
- [ ] Record the approved Admin framework change in `REHLA_DECISIONS.md` using the document's change-control fields: previous Admin choice, new Next.js App Router choice, reason, affected Admin/Web/UI/API boundaries, migration and compatibility impact, and required verification. Keep the Web Next.js decision and all Rehla ownership boundaries intact.
- [ ] Update architecture repository shape and Admin/Web sections to specify Next.js App Router; keep Admin resources and business behavior Rehla-owned.
- [ ] Update README architecture summary and plans index to link the foundation specification and plan.
- [ ] Add the framework decision to root/app agent guidance; do not alter module, provider, or SDK domain rules.
- [ ] Run: Select-String -Path docs/development/architecture.md,README.md,AGENTS.md,apps/AGENTS.md,apps/admin/AGENTS.md,apps/web/AGENTS.md -Pattern 'React/Vite-like'. Expected: no matches for the Admin target.
- [ ] Check each changed local Markdown link resolves with Test-Path; manually compare the repository shape and framework statements across the spec, architecture, README, and agent guidance.

### FND-02: Establish the single root Yarn workspace and absorb UI workspace configuration

**Files:**
- Create: package.json
- Create: yarn.lock
- Create: .yarnrc.yml
- Create: .node-version
- Create: .yarn/releases/yarn-3.5.1.cjs (move/copy the existing pinned Yarn release)
- Create: .yarn/patches/class-variance-authority-npm-0.6.1-22a468e86e.patch
- Create: .yarn/plugins/@yarnpkg/plugin-interactive-tools.cjs
- Create: .yarn/plugins/@yarnpkg/plugin-workspace-tools.cjs
- Create: apps/api/package.json
- Create: apps/admin/package.json
- Create: apps/web/package.json
- Create: packages/contracts/package.json
- Create: packages/sdk/package.json
- Modify: packages/ui/package.json
- Modify: packages/ui/configs/eslint-config-ui/package.json
- Modify: packages/ui/configs/tsconfig-ui/package.json
- Modify: packages/ui/packages/ui/package.json
- Modify: packages/ui/tests/smoke/package.json
- Modify: packages/ui/tools/figma-api/package.json
- Modify: packages/ui/tools/toolbox/package.json
- Remove: packages/ui/.yarnrc.yml
- Remove: packages/ui/yarn.lock
- Remove: packages/ui/turbo.json
- Remove: packages/ui/.yarn/releases/yarn-3.5.1.cjs after the root copy is verified
- Remove: packages/ui/.yarn/patches/class-variance-authority-npm-0.6.1-22a468e86e.patch after the root copy is verified
- Remove: packages/ui/.yarn/plugins/@yarnpkg/plugin-interactive-tools.cjs after the root copy is verified
- Remove: packages/ui/.yarn/plugins/@yarnpkg/plugin-workspace-tools.cjs after the root copy is verified
- Preserve: all UI component/config/tool/test source, .changeset data, and nested UI GitHub workflow source

**Interfaces:**
- Consumes: Existing UI workspace names and Yarn 3.5.1 lock resolutions.
- Produces: Root package manager and workspace patterns for apps/*, packages/contracts, packages/sdk, packages/modules/*, packages/providers/*, packages/ui, and the existing nested UI workspace folders.

- [ ] Create root package.json with packageManager yarn@3.5.1, engine requirement Node 24, one root workspaces list, root devDependency turbo 2.11.6, and root resolution for the existing class-variance-authority patch.
- [ ] Set workspaces to apps/*, packages/contracts, packages/sdk, packages/modules/*, packages/providers/*, packages/ui, packages/ui/configs/*, packages/ui/packages/*, packages/ui/tests/*, and packages/ui/tools/*.
- [ ] Copy the pinned Yarn release, required Yarn plugins, and patch from packages/ui/.yarn to root .yarn; update plugin/patch paths to be root-relative.
- [ ] Configure root .yarnrc.yml with nodeLinker: node-modules, nmHoistingLimits: workspaces, and the pinned yarnPath.
- [ ] Update packages/ui/package.json to remove its nested packageManager and workspaces root declarations; remove scripts that recursively invoke Turbo from inside the root Turbo graph.
- [ ] Change internal UI package dependencies from wildcard ranges to workspace:* where they refer to the listed local workspaces; keep external dependency ranges and public package names unchanged.
- [ ] Create minimal named manifests: @rehla/api, @rehla/admin, @rehla/web, @rehla/contracts, and @rehla/sdk so root discovery is complete before their implementation tasks.
- [ ] Set .node-version to 24.
- [ ] Run: corepack enable; yarn --version. Expected: 3.5.1.
- [ ] Run: yarn install. Expected: one root yarn.lock generated from all declared workspaces.
- [ ] Run: yarn install --immutable. Expected: successful root install and no nested packages/ui/yarn.lock generated.
- [ ] Run: yarn workspaces list --json. Expected: each root app, contracts/SDK package, UI container, and six existing UI leaf workspaces appears exactly once.
- [ ] In Task 3, add the repeatable discovery check and rerun it after all workspaces are present.

### FND-03: Define Turborepo tasks and repeatable workspace checks

**Files:**
- Create: turbo.json
- Create: scripts/verify-workspaces.mjs
- Create: .prettierignore
- Modify: package.json
- Modify: packages/ui/package.json

**Interfaces:**
- Consumes: Root Yarn workspace graph from Task 2.
- Produces: Root commands dev, build, lint, typecheck, test, format:check, and verify:workspaces; Turbo task graph with dependency-aware build/test and uncached persistent dev tasks.

- [ ] Migrate the UI Turbo pipeline to the Turborepo 2 tasks schema and define build outputs for dist/**, .next/**, and API build output; mark dev persistent and uncached.
- [ ] Define root scripts that invoke the root turbo binary once; do not retain a UI container script that recursively calls turbo run.
- [ ] Add root Prettier 3.9.6. Add root React/React DOM 19.2 and matching React type devDependencies for shared UI test tooling; apps also declare the same runtime versions.
- [ ] Add .prettierignore entries for node_modules, .next, .turbo, .yarn, and .medusa. Define format:check as prettier --check over apps/**/*.{js,mjs,ts,tsx,json,md}, packages/{contracts,sdk}/**/*.{js,mjs,ts,tsx,json,md}, packages/ui/**/package.json, README.md, AGENTS.md, and the changed development architecture/plan/spec files; never invoke the old write-mode format script.
- [ ] Create scripts/verify-workspaces.mjs. It must invoke yarn workspaces list --json, parse JSON lines, reject duplicate names, and assert the 12 expected workspace names from Task 2.
- [ ] Add a workspace verification script command and document its expected workspace set in the script itself.
- [ ] Run: yarn verify:workspaces. Expected: exit code 0 and all expected workspace names reported.
- [ ] Run: yarn turbo run build --dry=json and yarn turbo run test --dry=json. Expected: valid JSON task graphs, upstream UI build before consumers, and no cycle diagnostics.
- [ ] Run: yarn format:check. Expected: no formatting changes; this command must check only supported file types and must not write files.

### FND-04: Create API bootstrap, local PostgreSQL, health contract, and SDK client

**Files:**
- Create: apps/api/medusa-config.ts
- Create: apps/api/tsconfig.json
- Create: apps/api/src/api/health/route.ts
- Create: apps/api/integration-tests/http/health.spec.ts
- Create: apps/api/jest.config.cjs
- Create: apps/api/integration-tests/setup.cjs
- Create: apps/api/.env.example
- Create: packages/contracts/package.json
- Create: packages/contracts/tsconfig.json
- Create: packages/contracts/src/health.ts
- Create: packages/sdk/package.json
- Create: packages/sdk/tsconfig.json
- Create: packages/sdk/src/client.ts
- Create: packages/sdk/src/client.test.ts
- Create: packages/sdk/vitest.config.ts
- Create: compose.yaml
- Modify: apps/api/package.json
- Modify: package.json
- Modify: turbo.json

**Interfaces:**
- Consumes: @rehla/contracts exports HealthResponse = { status: "ok"; service: "api" }.
- Produces: @rehla/sdk exports createRehlaClient({ baseUrl, fetchImpl? }) with getHealth(): Promise<HealthResponse>; API GET /health returns the same JSON with HTTP 200.

- [ ] Add @rehla/contracts as a TypeScript workspace package and export HealthResponse from its package root.
- [ ] Add @rehla/sdk as a transport-only workspace package. Define createRehlaClient options as { baseUrl: string; fetchImpl?: typeof fetch }; getHealth performs GET to baseUrl + /health, rejects non-2xx responses, and returns parsed HealthResponse.
- [ ] Write SDK unit tests first for successful response, non-2xx response, fetch rejection, and malformed response body; run yarn workspace @rehla/sdk test and confirm the new tests fail before implementation.
- [ ] Implement the SDK client and rerun yarn workspace @rehla/sdk test. Expected: all four cases pass.
- [ ] Bootstrap apps/api from the Medusa 2.21.2 application runtime without importing Medusa Admin UI or adding a Medusa source tree.
- [ ] Implement unauthenticated `GET /health` in `apps/api/src/api/health/route.ts`; return HTTP 200 and exactly `{ status: "ok", service: "api" }` with JSON content type.
- [ ] Add an HTTP integration test that starts the configured API test application, issues `GET /health`, and asserts status 200 and the exact JSON body; run `yarn workspace @rehla/api test:integration:http` with a 60-second Jest timeout.
- [ ] Add `jest.config.cjs` and `integration-tests/setup.cjs`. Use `medusaIntegrationTestRunner` from the pinned `@medusajs/test-utils` public export to launch the app; declare Jest 29.7, `@swc/jest`, `@swc/core`, `@types/jest`, and `cross-env` 7.0.3. Define the cross-platform command as `cross-env TEST_TYPE=integration:http NODE_OPTIONS=--experimental-vm-modules jest --silent=false --runInBand --forceExit`.
- [ ] Add compose.yaml for local PostgreSQL 17 only, with persistent named volume, healthcheck, localhost-only port binding, and development-only credentials read from the API .env.example.
- [ ] Set API database configuration through DATABASE_URL; .env.example must contain a clearly local-only sample and no production credentials.
- [ ] Run: docker compose up -d postgres; yarn workspace @rehla/api build; yarn workspace @rehla/api test:integration:http. Expected: database healthy, API build succeeds, and the route integration test passes.
- [ ] Run the API locally and request http://localhost:9000/health. Expected: HTTP 200 and the exact HealthResponse JSON.
- [ ] Do not create a root tsconfig consumed by apps/api; each workspace owns a tsconfig to remain available in a Medusa Turborepo-pruned build.

### FND-05: Create Next.js Admin and Web shells that consume shared UI

**Files:**
- Create: apps/admin/next.config.ts
- Create: apps/admin/tsconfig.json
- Create: apps/admin/eslint.config.mjs
- Create: apps/admin/src/app/layout.tsx
- Create: apps/admin/src/app/page.tsx
- Create: apps/admin/src/app/globals.css
- Create: apps/web/next.config.ts
- Create: apps/web/tsconfig.json
- Create: apps/web/eslint.config.mjs
- Create: apps/web/src/app/layout.tsx
- Create: apps/web/src/app/page.tsx
- Create: apps/web/src/app/globals.css
- Modify: apps/admin/package.json
- Modify: apps/web/package.json
- Modify: packages/ui/packages/ui/package.json
- Modify: turbo.json

**Interfaces:**
- Consumes: @rehla-ui/ui Button and styles.css exports; @rehla/sdk client boundary; shared Next.js 16.2 and compatible React peer versions.
- Produces: Independently startable and buildable Admin (port 3001) and Web (port 3000) App Router shells.

- [ ] Add Next.js 16.2 App Router dependencies and scripts to both app manifests; use React 19.2 and pin identical Next.js and React versions in the root lockfile.
- [ ] Add @rehla-ui/ui and @rehla/sdk as workspace dependencies in both apps; enable transpilation for the shared UI package where required by Next.js.
- [ ] Extend @rehla-ui/ui React and React DOM peer ranges to ^18.0.0 || ^19.2.0; remove workspace-local React runtime devDependencies in favor of the root React 19.2 test versions; update React type packages to 19.2, @testing-library/react to 16.3.3, and @testing-library/dom to 10.x while preserving all component exports.
- [ ] Run: yarn workspace @rehla-ui/ui test; yarn workspace @rehla-ui/ui build. Expected: all existing component/API tests pass under React 19.2 and public exports remain unchanged.
- [ ] Create Admin layout and page with a branded empty-state heading and one generic Button import from @rehla-ui/ui.
- [ ] Create Web layout and page with a branded empty-state heading and one generic Button import from @rehla-ui/ui.
- [ ] Import the UI package's exported styles.css in each app and keep app-specific styles local.
- [ ] Add independent dev/build/lint/typecheck scripts; use eslint directly because Next.js 16 removed next lint. Do not add empty app test scripts before an app behavior exists.
- [ ] Run: yarn workspace @rehla/admin build; yarn workspace @rehla/web build. Expected: both production builds succeed with no duplicate-React or missing CSS-export errors.
- [ ] Run: yarn dev; open http://localhost:3001 and http://localhost:3000. Expected: both placeholder shells render and the shared UI button is styled.
- [ ] Confirm apps/admin and apps/web import no apps/api or packages/modules source paths.

### FND-06: Add CI and clean-checkout developer instructions

**Files:**
- Create: .github/workflows/ci.yml
- Modify: README.md for runtime/setup/commands
- Modify: package.json
- Modify: docs/development/plans/README.md
- Modify: apps/api/.env.example
- Preserve: docs/development/constitution.md and docs/development/decisions/001-medusa-without-copying.md

**Interfaces:**
- Consumes: Root scripts and app/runtime commands from Tasks 2–5.
- Produces: CI and developer setup that use the same root install, validation, and app commands.

- [ ] Configure CI for pushes and pull requests using actions/checkout@v4, actions/setup-node@v4, Node.js 24, Corepack, Yarn 3.5.1, immutable root install, and PostgreSQL 17 service.
- [ ] Run CI steps in this order: verify:workspaces, lint, typecheck, test (including the Medusa health-route integration test), and build.
- [ ] Cache dependencies from the root yarn.lock; do not cache or install from packages/ui as a separate project.
- [ ] Document prerequisites, Corepack setup, root immutable install, Docker Compose PostgreSQL, all root scripts, focused workspace commands, local ports, and .env.example copying.
- [ ] Document that Admin and Web are Next.js App Router apps and API is a separate Medusa runtime owned by Rehla.
- [ ] Add the Medusa testing tool setup files and cross-platform test command using cross-env so the HTTP integration test runs on Windows and CI.
- [ ] Check README and plans index links; run yarn format:check and all CI commands locally. Expected: same commands and configuration shape as CI.

### FND-07: Converge the foundation and record the handoff

**Files:**
- Modify: docs/development/plans/00-foundation.md

**Interfaces:**
- Consumes: Completed implementation evidence for Tasks 1–6.
- Produces: A traceable acceptance record and a clean handoff for the next approved plan.

- [ ] Run from the repository root: yarn install --immutable; yarn verify:workspaces; yarn lint; yarn typecheck; yarn test; yarn build.
- [ ] Run the API health smoke request and verify both Next.js apps manually at their documented local ports.
- [ ] Run yarn turbo prune @rehla/api --docker and verify the pruned API build contains all required workspace manifests/configuration without relying on arbitrary root files.
- [ ] Compare each of the ten Foundation Acceptance Criteria above against an implementation task and captured command/manual evidence.
- [ ] Record automated results and human manual review separately; leave manual criteria unchecked until confirmed by a person.
- [ ] Record exact progress, decisions, discoveries, validation, and remaining blockers in this plan; do not claim completion while any acceptance criterion remains open.

## Locked Foundation Decisions

- Runtime/tool versions, workspace patterns, API health schema, local PostgreSQL behavior, UI peer compatibility, app boundaries, root scripts, and CI commands are specified in this plan's Global Constraints and Tasks FND-01–FND-07.
- The Medusa runtime is an npm dependency at the pinned version stated above. Its repository, dashboard, modules, and source tree are not copied into Rehla.
- No implementation task may infer extra Rehla business domains from the runtime's available features.

---

## Execution Order

Tasks are dependency-ordered: Task 1 → Task 2 → Task 3 → Task 4 → Task 5 → Task 6 → Task 7. Keep the implementation in this order so the approved architecture and workspace graph exist before app code depends on them.

## Completion Gate

Foundation is complete only when all acceptance criteria in the approved specification have implementation evidence, the root workspace and CI checks pass, the API health endpoint and both frontend shells have passed manual review, and no Rehla business domain or Medusa repository copy has entered this scope.

