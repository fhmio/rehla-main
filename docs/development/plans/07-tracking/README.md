# Tracking Plan Index

## Objective

Provide immutable operational timelines and customer/admin tracking views for applications and transactions.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `07-01-status-model` | Tracking Status Model | 04-03-application-lifecycle, 06-05-wallet-transaction-lifecycle | DRAFT |
| `07-02-application-timeline` | Application Timeline | 07-01-status-model | DRAFT |
| `07-03-customer-admin-tracking` | Customer and Admin Tracking Views | 07-02-application-timeline, 04-04-customer-application-access, 04-05-admin-application-review | DRAFT |

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
