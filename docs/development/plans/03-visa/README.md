# Visa Catalog Plan Index

## Objective

Provide Rehla service catalog capabilities for visas, tourist visits, and Umrah services.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `03-01-visa-categories` | Visa Service Categories | 00-03-domain-package-boundaries | DRAFT |
| `03-02-visa-services` | Visa Service Catalog | 03-01-visa-categories | DRAFT |
| `03-03-service-requirements` | Service Requirements | 03-02-visa-services | DRAFT |
| `03-04-service-publishing-media` | Service Publishing and Media | 03-02-visa-services | DRAFT |
| `03-05-public-service-discovery` | Public Service Discovery | 03-02-visa-services, 03-04-service-publishing-media | DRAFT |

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
