# Foundation Plan Index

## Objective

Establish the Rehla monorepo, application/package boundaries, shared contracts, API contract baseline, and quality gates required before domain feature implementation.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `00-01-monorepo-foundation` | Monorepo Foundation | — | DRAFT |
| `00-02-application-foundation` | Application Runtime Foundations | 00-01-monorepo-foundation | DRAFT |
| `00-03-domain-package-boundaries` | Domain Package Boundaries | 00-01-monorepo-foundation | DRAFT |
| `00-04-shared-sdk-ui-config` | Shared SDK, UI, and Config Foundations | 00-02-application-foundation, 00-03-domain-package-boundaries | DRAFT |
| `00-05-api-contract-foundation` | API Contract Foundation | 00-02-application-foundation, 00-03-domain-package-boundaries | DRAFT |
| `00-06-testing-quality-gates` | Testing and Quality Gates | 00-01-monorepo-foundation | DRAFT |

## Feature Bundle Contract

Every feature directory has exactly:

```text
<feature>/
├── spec.md
├── checklist.md
├── plan.md
└── tasks.md
```

The files are intentionally identical in structure across all domains. Feature-specific content is filled from the approved requirements, domain boundary, dependency graph, and technical plan.
