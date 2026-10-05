# Notifications Plan Index

## Objective

Deliver in-app notifications and provider-backed messaging as event-driven side effects.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `08-01-notification-domain` | Notification Domain | 01-01-customer-registration, 00-03-domain-package-boundaries | DRAFT |
| `08-02-event-driven-notifications` | Event-Driven Notifications | 08-01-notification-domain, 04-03-application-lifecycle, 06-05-wallet-transaction-lifecycle | DRAFT |
| `08-03-notification-read-state` | Notification Read State | 08-02-event-driven-notifications | DRAFT |

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
