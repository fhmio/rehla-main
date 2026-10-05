# Web Plan Index

## Objective

Build the customer-facing web experience over the public and authenticated API contracts.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `10-01-homepage-and-banners` | Homepage and Banners | 03-05-public-service-discovery, 00-04-shared-sdk-ui-config | DRAFT |
| `10-02-service-discovery` | Service Discovery | 03-05-public-service-discovery | DRAFT |
| `10-03-service-details` | Service Details | 10-02-service-discovery, 03-03-service-requirements | DRAFT |
| `10-04-application-flow` | Application Flow | 04-02-create-application, 05-02-secure-document-upload | DRAFT |
| `10-05-wallet-and-payment-flow` | Wallet and Payment Flow | 06-03-bank-transfer-funding, 06-04-instant-payment-providers, 06-08-wallet-history-and-balance | DRAFT |
| `10-06-tracking-notifications-profile` | Tracking, Notifications, and Profile | 07-03-customer-admin-tracking, 08-03-notification-read-state, 02-01-customer-profile | DRAFT |

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
