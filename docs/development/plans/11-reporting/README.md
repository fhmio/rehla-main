# Reporting Plan Index

## Objective

Provide reporting views over existing domain data without introducing a separate analytics architecture.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `11-01-application-reporting` | Application Reporting | 04-06-application-history | DRAFT |
| `11-02-payment-reporting` | Payment Reporting | 06-05-wallet-transaction-lifecycle | DRAFT |
| `11-03-customer-reporting` | Customer Reporting | 02-02-admin-customer-management | DRAFT |
| `11-04-dashboard-reporting-integration` | Dashboard Reporting Integration | 11-01-application-reporting, 11-02-payment-reporting, 11-03-customer-reporting, 09-03-admin-dashboard | DRAFT |

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
