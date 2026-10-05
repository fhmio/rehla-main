# Rehla Implementation Roadmap

## Execution Rule

Implement capability-level features in dependency order. A feature becomes `READY` only after `spec.md`, `checklist.md`, `plan.md`, `tasks.md`, and cross-artifact analysis are complete. Large features may be executed in phases, with validation after each phase.

## Domain Sequence

```text
00 Foundation
   ↓
01 Auth
   ↓
02 Users
   ↓
03 Visa
   ↓
04 Applications
   ↓
05 Documents
   ↓
06 Payments
   ↓
07 Tracking + 08 Notifications
   ↓
09 Admin + 10 Web
   ↓
11 Reporting
```

The domain sequence is a dependency map, not a requirement to keep each domain as one large agent task.

## Domain Feature Matrix

### 00-foundation — Foundation

| Feature | Depends on | Outcome |
|---|---|---|
| `00-01-monorepo-foundation` | — | Create the pnpm workspace and Turborepo task graph with the fixed apps/packages boundary. |
| `00-02-application-foundation` | 00-01-monorepo-foundation | Establish the web, admin, and api application boundaries and their repository contracts. |
| `00-03-domain-package-boundaries` | 00-01-monorepo-foundation | Establish isolated domain module packages and public entrypoint boundaries. |
| `00-04-shared-sdk-ui-config` | 00-02-application-foundation, 00-03-domain-package-boundaries | Establish shared client SDK, UI primitives, and configuration packages without moving business logic into them. |
| `00-05-api-contract-foundation` | 00-02-application-foundation, 00-03-domain-package-boundaries | Establish the REST API boundary and OpenAPI as the authoritative HTTP contract. |
| `00-06-testing-quality-gates` | 00-01-monorepo-foundation | Establish automated test, lint, typecheck, build, and validation gates for packages and applications. |

### 01-auth — Auth

| Feature | Depends on | Outcome |
|---|---|---|
| `01-01-customer-registration` | 00-05-api-contract-foundation, 00-06-testing-quality-gates | Allow a customer to create an account through the authenticated platform boundary. |
| `01-02-login-session-management` | 01-01-customer-registration | Implement login, access tokens, refresh sessions, rotation, revocation, logout, and protected-session behavior. |
| `01-03-account-recovery-verification` | 01-02-login-session-management | Implement the defined verification and recovery paths without exposing credentials or sensitive data. |
| `01-04-role-based-access` | 01-02-login-session-management | Enforce CUSTOMER and ADMIN role boundaries and protect admin operations. |

### 02-users — Users

| Feature | Depends on | Outcome |
|---|---|---|
| `02-01-customer-profile` | 01-01-customer-registration | Create and maintain the customer profile and editable account data. |
| `02-02-admin-customer-management` | 01-04-role-based-access, 02-01-customer-profile | Allow authorized admins to search, view, and manage customer accounts. |

### 03-visa — Visa Catalog

| Feature | Depends on | Outcome |
|---|---|---|
| `03-01-visa-categories` | 00-03-domain-package-boundaries | Define and manage the categories used to organize Rehla services. |
| `03-02-visa-services` | 03-01-visa-categories | Create the service model containing title, description, price, currency, status, and service-specific data. |
| `03-03-service-requirements` | 03-02-visa-services | Define requirements and required documents associated with each service. |
| `03-04-service-publishing-media` | 03-02-visa-services | Manage service publication state and associated images/media. |
| `03-05-public-service-discovery` | 03-02-visa-services, 03-04-service-publishing-media | Expose published services for customer discovery, filtering, and detail viewing. |

### 04-applications — Applications

| Feature | Depends on | Outcome |
|---|---|---|
| `04-01-applicant-data` | 02-01-customer-profile, 03-02-visa-services | Capture applicant/service-specific data required to start an application. |
| `04-02-create-application` | 04-01-applicant-data, 03-03-service-requirements | Create an application from a published service and a valid applicant context. |
| `04-03-application-lifecycle` | 04-02-create-application | Define and enforce application status transitions and transition validation. |
| `04-04-customer-application-access` | 04-02-create-application, 04-03-application-lifecycle, 01-02-login-session-management | Allow customers to list, view, and monitor only their own applications. |
| `04-05-admin-application-review` | 04-03-application-lifecycle, 01-04-role-based-access | Provide authorized admin review, status actions, notes, and requests for customer action. |
| `04-06-application-history` | 04-03-application-lifecycle | Preserve accepted status transitions and operational history needed for tracking and audit. |

### 05-documents — Documents

| Feature | Depends on | Outcome |
|---|---|---|
| `05-01-document-requirements` | 03-03-service-requirements, 04-02-create-application | Instantiate the documents required by a selected service for an application. |
| `05-02-secure-document-upload` | 05-01-document-requirements | Upload application documents with server-controlled storage keys and validation. |
| `05-03-document-review` | 05-02-secure-document-upload, 01-04-role-based-access | Allow authorized admins to accept, reject, or request a replacement document. |
| `05-04-document-version-history` | 05-03-document-review | Preserve previous submissions while allowing re-upload and review of a new version. |

### 06-payments — Payments

| Feature | Depends on | Outcome |
|---|---|---|
| `06-01-wallet-domain` | 02-01-customer-profile, 00-03-domain-package-boundaries | Create the customer wallet, balance representation, state, and transaction ownership boundary. |
| `06-02-wallet-funding-request` | 06-01-wallet-domain | Create a funding transaction from a customer-selected amount and enabled payment method. |
| `06-03-bank-transfer-funding` | 06-02-wallet-funding-request | Support bank selection, operation number, receipt evidence, and manual-review state for wallet funding. |
| `06-04-instant-payment-providers` | 06-02-wallet-funding-request | Integrate the enabled instant-payment paths for Bravo and CashiPay behind provider boundaries. |
| `06-05-wallet-transaction-lifecycle` | 06-03-bank-transfer-funding, 06-04-instant-payment-providers | Enforce pending, review, approval, completion, rejection, failure, and retry/idempotency rules. |
| `06-06-payment-review` | 06-03-bank-transfer-funding, 01-04-role-based-access | Allow authorized admins to review, approve, or reject funding evidence. |
| `06-07-banks-and-payment-methods` | 06-03-bank-transfer-funding, 06-04-instant-payment-providers | Manage enabled banks, transfer account details, and payment-method availability. |
| `06-08-wallet-history-and-balance` | 06-05-wallet-transaction-lifecycle | Expose wallet balance and transaction history to the customer with correct ownership and states. |

### 07-tracking — Tracking

| Feature | Depends on | Outcome |
|---|---|---|
| `07-01-status-model` | 04-03-application-lifecycle, 06-05-wallet-transaction-lifecycle | Define the tracking representations for application and transaction progress without conflating their state machines. |
| `07-02-application-timeline` | 07-01-status-model | Record immutable status-history entries for application transitions. |
| `07-03-customer-admin-tracking` | 07-02-application-timeline, 04-04-customer-application-access, 04-05-admin-application-review | Provide appropriate timeline views for customers and operational staff. |

### 08-notifications — Notifications

| Feature | Depends on | Outcome |
|---|---|---|
| `08-01-notification-domain` | 01-01-customer-registration, 00-03-domain-package-boundaries | Own notification records, templates/metadata, recipients, channels, and delivery state. |
| `08-02-event-driven-notifications` | 08-01-notification-domain, 04-03-application-lifecycle, 06-05-wallet-transaction-lifecycle | Subscribe to business events and create the required customer notifications. |
| `08-03-notification-read-state` | 08-02-event-driven-notifications | Allow customers to view and mark in-app notifications as read. |

### 09-admin — Admin

| Feature | Depends on | Outcome |
|---|---|---|
| `09-01-admin-shell-and-navigation` | 00-04-shared-sdk-ui-config, 01-04-role-based-access | Create the reusable admin shell, navigation, header, responsive behavior, and access-aware navigation. |
| `09-02-admin-acl` | 01-04-role-based-access | Expose permissions for resource access and sensitive operational actions. |
| `09-03-admin-dashboard` | 09-01-admin-shell-and-navigation, 04-05-admin-application-review, 06-06-payment-review | Provide operational dashboard widgets for applications, payments, customers, revenue, and queues. |
| `09-04-admin-visa-management` | 03-02-visa-services, 03-03-service-requirements, 09-02-admin-acl | Provide service/category/requirement CRUD and publishing controls. |
| `09-05-admin-application-management` | 04-05-admin-application-review, 05-03-document-review | Provide application lists, detail review, document review, status actions, and notes. |
| `09-06-admin-payment-management` | 06-06-payment-review, 06-07-banks-and-payment-methods | Provide wallet, transactions, transfer review, bank, and payment method administration. |
| `09-07-admin-content-and-users` | 02-02-admin-customer-management, 09-04-admin-visa-management | Provide customer and banner/content management surfaces. |
| `09-08-admin-audit-and-reports` | 04-06-application-history, 06-06-payment-review | Expose authorized operational audit views and the reporting surfaces defined for Rehla. |

### 10-web — Web

| Feature | Depends on | Outcome |
|---|---|---|
| `10-01-homepage-and-banners` | 03-05-public-service-discovery, 00-04-shared-sdk-ui-config | Display promotional banners and entry points to supported Rehla services. |
| `10-02-service-discovery` | 03-05-public-service-discovery | Provide category navigation, search/filtering where specified, and service lists. |
| `10-03-service-details` | 10-02-service-discovery, 03-03-service-requirements | Display price, currency, description, requirements, and start-application action. |
| `10-04-application-flow` | 04-02-create-application, 05-02-secure-document-upload | Implement customer-facing applicant data, document submission, review, and application creation flow. |
| `10-05-wallet-and-payment-flow` | 06-03-bank-transfer-funding, 06-04-instant-payment-providers, 06-08-wallet-history-and-balance | Implement customer wallet display, charge flow, and payment-method selection defined for wallet funding. |
| `10-06-tracking-notifications-profile` | 07-03-customer-admin-tracking, 08-03-notification-read-state, 02-01-customer-profile | Provide customer status tracking, notifications, and profile/account views. |

### 11-reporting — Reporting

| Feature | Depends on | Outcome |
|---|---|---|
| `11-01-application-reporting` | 04-06-application-history | Report on application volumes, states, and time-based operational measures. |
| `11-02-payment-reporting` | 06-05-wallet-transaction-lifecycle | Report on wallet funding transactions, approvals, rejections, and amounts. |
| `11-03-customer-reporting` | 02-02-admin-customer-management | Report on customer counts and activity measures required by the dashboard. |
| `11-04-dashboard-reporting-integration` | 11-01-application-reporting, 11-02-payment-reporting, 11-03-customer-reporting, 09-03-admin-dashboard | Connect approved report data to admin dashboard widgets using the API/SDK boundary. |

## Parallel Execution Rule

Only features with independent dependencies, non-overlapping ownership, and independent validation may be marked `[P]` in `tasks.md`. Domain-level ordering does not itself imply a parallelization decision.

## Completion Rule

A domain is complete only after every feature under the domain reaches `COMPLETED` or has a documented explicit exclusion. Reporting consumes approved domain data and must not introduce a second analytics architecture.
