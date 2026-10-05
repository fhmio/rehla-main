# Auth Plan Index

## Objective

Provide customer/admin authentication and session security as an isolated domain capability.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `01-01-customer-registration` | Customer Registration | 00-05-api-contract-foundation, 00-06-testing-quality-gates | DRAFT |
| `01-02-login-session-management` | Login and Session Management | 01-01-customer-registration | DRAFT |
| `01-03-account-recovery-verification` | Account Recovery and Verification | 01-02-login-session-management | DRAFT |
| `01-04-role-based-access` | Role-Based Access Control | 01-02-login-session-management | DRAFT |

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
