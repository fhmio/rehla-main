# Applications Plan Index

## Objective

Own the customer application lifecycle from creation through processing and completion.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `04-01-applicant-data` | Applicant Data | 02-01-customer-profile, 03-02-visa-services | DRAFT |
| `04-02-create-application` | Create Application | 04-01-applicant-data, 03-03-service-requirements | DRAFT |
| `04-03-application-lifecycle` | Application Lifecycle | 04-02-create-application | DRAFT |
| `04-04-customer-application-access` | Customer Application Access | 04-02-create-application, 04-03-application-lifecycle, 01-02-login-session-management | DRAFT |
| `04-05-admin-application-review` | Admin Application Review | 04-03-application-lifecycle, 01-04-role-based-access | DRAFT |
| `04-06-application-history` | Application History | 04-03-application-lifecycle | DRAFT |

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
