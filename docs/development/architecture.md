# Rehla Architecture Baseline

## Status

BASELINE — 2026-10-03

## Repository Shape

```text
rehla/
├── apps/
│   ├── web/
│   ├── admin/
│   └── api/
│
├── packages/
│   ├── modules/
│   │   ├── auth/
│   │   ├── users/
│   │   ├── visa/
│   │   ├── applications/
│   │   ├── documents/
│   │   ├── payments/
│   │   ├── tracking/
│   │   ├── banners/
│   │   ├── notifications/
│   │   └── audit/
│   ├── providers/
│   ├── sdk/
│   ├── ui/
│   └── config/
│
├── integration-tests/
├── scripts/
└── docs/development/
```

## Runtime Direction

```text
Web ──────┐
          ├──> @rehla/sdk ───> API
Admin ────┘                     │
                                ▼
                           API Routes
                                │
                                ▼
                            Workflows
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                 Modules     Query/Links   Providers
```

## Applications

### Web

Customer-facing application. It owns rendering, navigation, form UX, and customer interactions. It does not access domain persistence directly.

### Admin

Operational application. It consumes the same API boundary through the SDK and applies admin-specific UX and authorization-aware navigation.

### API

Backend runtime. It owns HTTP routes, workflows, subscribers, scheduled jobs, links/associations, and runtime configuration.

## Domain Modules

The project uses these business boundaries:

- Auth — identity/session behavior.
- Users — customer profiles/account data.
- Visa — services, categories, requirements, publication.
- Applications — customer service applications and lifecycle.
- Documents — required documents, uploads, review, versions.
- Payments — wallet and wallet-funding transactions.
- Tracking — timeline/status presentation and history records.
- Notifications — notification records and channel delivery state.
- Banners — promotional content and click destinations.
- Audit — operational audit records for sensitive actions.

## Providers

Providers are for systems external to the core domain:

```text
Payment Provider
Storage Provider
Notification Provider
```

Only integrations required by the current product scope are represented in plans.

## Business Orchestration

A route does not own a multi-domain business transaction. A workflow coordinates domain operations, for example:

```text
Payment Review
  → Payment
  → Wallet
  → Notification
  → Audit
```

and:

```text
Application Review
  → Applications
  → Tracking
  → Notification
  → Audit
```

## Cross-Domain Access

Modules remain isolated. The approved access patterns are:

- Query/read composition for cross-domain reads.
- Explicit links/associations for cross-domain relationships.
- Workflows for multi-domain business orchestration.
- Subscribers for asynchronous side effects.

## HTTP Contract

The REST API uses `/api/v1` as its stable base path. OpenAPI is authoritative for request/response shapes. The exact endpoint inventory belongs in feature plans and the generated API contract.

## Current Product Scope Evidence

The current Rehla requirements source states that the platform covers visas, tourist visits, Umrah, applications, documents, tracking, internal wallet, wallet funding, Bravo, CashiPay, Bank Transfer, bank-transfer review, bank management, payment methods, and operational administration. It explicitly excludes flight booking, hotel booking, and Marketplace/agency offers. fileciteturn106file0L1-L20 fileciteturn106file2L1-L12

The same source states that wallet-funding is defined, but using wallet balance to pay for visa/service applications is not an approved decision in that document. fileciteturn106file4L1-L20

## Architecture Reference Pattern

The project applies selected patterns from Medusa rather than cloning Medusa: modular domain packages, module isolation, workflows, query composition, links, providers, subscribers/jobs, shared SDK, and package/integration testing. Generic commerce modules and a generic commerce framework are deliberately outside the Rehla architecture.
