# Users Plan Index

## Objective

Own user profile data and customer-facing account operations, separate from authentication credentials.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `02-01-customer-profile` | Customer Profile | 01-01-customer-registration | DRAFT |
| `02-02-admin-customer-management` | Admin Customer Management | 01-04-role-based-access, 02-01-customer-profile | DRAFT |

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
