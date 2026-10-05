# Admin Plan Index

## Objective

Build the Rehla administrative runtime as a consumer of the API/SDK, with operational workflows, ACL, and consistent UI patterns.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `09-01-admin-shell-and-navigation` | Admin Shell and Navigation | 00-04-shared-sdk-ui-config, 01-04-role-based-access | DRAFT |
| `09-02-admin-acl` | Admin ACL | 01-04-role-based-access | DRAFT |
| `09-03-admin-dashboard` | Admin Dashboard | 09-01-admin-shell-and-navigation, 04-05-admin-application-review, 06-06-payment-review | DRAFT |
| `09-04-admin-visa-management` | Admin Visa Service Management | 03-02-visa-services, 03-03-service-requirements, 09-02-admin-acl | DRAFT |
| `09-05-admin-application-management` | Admin Application Management | 04-05-admin-application-review, 05-03-document-review | DRAFT |
| `09-06-admin-payment-management` | Admin Payment Management | 06-06-payment-review, 06-07-banks-and-payment-methods | DRAFT |
| `09-07-admin-content-and-users` | Admin Content and User Management | 02-02-admin-customer-management, 09-04-admin-visa-management | DRAFT |
| `09-08-admin-audit-and-reports` | Admin Audit and Reporting | 04-06-application-history, 06-06-payment-review | DRAFT |

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
