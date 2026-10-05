# Rehla Architecture

## 1. Purpose

This document defines the target architecture for **Rehla (رحله)** as a Rehla-owned modular commerce/service platform. It adopts the relevant architectural philosophy and proven boundaries observed in the supplied Medusa source, while avoiding a Medusa fork, repository copy, or unrelated commerce behavior.

The architecture is intentionally centered on Rehla's actual scope:

- Visa services, including Umrah and tourist visas for Gulf destinations.
- Customers and customer authentication.
- Visa applications.
- Applicant documents and file storage.
- Bank-transfer payment and receipt review.
- Application tracking.
- Notifications.
- Storefront content such as banners.
- Admin users, permissions, reporting, and operational administration.

The following are outside the current domain boundary:

- Flight booking.
- Hotel booking.
- Travel-agency marketplace.
- Shipping and fulfillment logistics.
- Inventory/stock-location operations required by physical commerce.

---

## 2. Architectural Principles

### 2.1 Rehla owns the architecture

Rehla is an independent monorepo. Medusa is a reference implementation and a source of compatible building blocks/patterns, not the application that owns the project.

### 2.2 Modular monolith

Backend business capabilities are isolated into modules inside one deployable application boundary. Modules communicate through explicit service contracts, workflows, events, and links rather than direct access to another module's internal implementation.

### 2.3 Domain first, framework second

Rehla terminology is authoritative. Where a Medusa concept maps cleanly, the architectural pattern may be retained. Domain names should not be changed merely to mirror Medusa internals.

### 2.4 Smallest compatible reuse

Reuse should occur at the smallest unit that already matches Rehla behavior. A compatible service, provider contract, UI primitive, SDK convention, or framework mechanism is preferable to importing an entire unrelated domain subsystem.

### 2.5 Backend before surfaces

The order of implementation is:

```text
model/schema
    ↓
module service
    ↓
workflow / event / link
    ↓
API contract
    ↓
Admin / Web integration
    ↓
automated verification
    ↓
manual verification
```

### 2.6 Explicit negative scope

Excluded commerce capabilities must not enter the architecture through accidental reuse. In particular, Rehla should not inherit Medusa concepts for shipping, fulfillment, inventory, physical stock locations, or marketplace behavior when those concepts do not exist in Rehla's requirements.

---

## 3. Repository Shape

```text
rehla/
├── apps/
│   ├── api/
│   │   └── src/
│   │       ├── api/                 # HTTP/API routes and composition
│   │       ├── features/            # app-level features such as banners/content
│   │       ├── workflows/           # cross-module business processes
│   │       ├── subscribers/         # event consumers
│   │       ├── jobs/                # scheduled/background work
│   │       ├── links/               # cross-module relationships
│   │       ├── search/              # search index definitions/composition
│   │       ├── feature-flags/       # application feature configuration
│   │       └── loaders/             # application startup composition
│   │
│   ├── admin/                       # React/Vite-like operational dashboard
│   └── web/                         # customer-facing storefront
│
├── packages/
│   ├── modules/
│   │   ├── store/
│   │   ├── customer/
│   │   ├── user/
│   │   ├── auth/
│   │   ├── rbac/
│   │   ├── product/
│   │   ├── cart/
│   │   ├── pricing/
│   │   ├── currency/
│   │   ├── region/
│   │   ├── application/
│   │   ├── documents/
│   │   ├── tracking/
│   │   ├── payment/
│   │   ├── notification/
│   │   ├── file/
│   │   ├── translation/
│   │   ├── search/
│   │   ├── settings/
│   │   ├── analytics/
│   │   ├── api-key/
│   │   ├── event-bus-*/
│   │   ├── workflow-engine-*/
│   │   ├── locking/
│   │   ├── caching/
│   │   └── link-modules/
│   │
│   ├── ui/                          # generic Rehla design-system primitives only
│   ├── icons/
│   ├── sdk/
│   ├── contracts/
│   └── config/
│
├── integration-tests/
├── docs/
└── scripts/
```

`@rehla/ui` is a generic design system. Domain resources such as Visa, Applications, Documents, Payments, Tracking, Customers, and Admin Users remain Admin resources/features inside `apps/admin`.

---

## 4. Application Boundaries

### 4.1 API application

`apps/api` is the application composition layer. It exposes HTTP APIs and assembles modules, workflows, subscribers, jobs, links, search, and startup loaders.

The API application should not become the owner of domain persistence that belongs inside modules. Application-level code coordinates capabilities; module services own their business entities and operations.

### 4.2 Admin application

`apps/admin` is the operational interface for Rehla staff. It follows the useful structural patterns from the supplied Medusa dashboard source:

- route-based resources
- query/data hooks
- table/list/detail patterns
- permissions/ability guards
- configurable layouts where appropriate
- extensible resource zones where required
- shared UI primitives without moving domain ownership into the design system

The Admin is Rehla-specific even when its shell or interaction patterns are Medusa-inspired.

### 4.3 Web application

`apps/web` is the customer-facing storefront. It consumes the Rehla API/contracts and presents Visa services, cart/application flows, payment instructions, tracking, account/profile, and content.

---

## 5. Module Taxonomy

### 5.1 Core business/commerce modules

| Module | Rehla responsibility | Mapping to Medusa philosophy |
|---|---|---|
| `store` | Rehla store/commerce context | Keep/adapt |
| `customer` | Customer identity/profile used by storefront | Keep/adapt |
| `user` | Admin/staff identity | Keep/adapt |
| `auth` | Authentication/provider boundary | Keep/adapt |
| `rbac` | Roles, permissions, access control | Keep/adapt |
| `product` | Visa service catalog; Product = Visa Service | Keep/adapt |
| `pricing` | Prices and pricing rules needed for services | Keep/adapt |
| `currency` | Currency definitions and conversions/context | Keep/adapt |
| `region` | Destination/country context | Keep/adapt |
| `cart` | Service cart containing Visa products | Adapt |
| `payment` | Payment abstraction and provider contract | Adapt |
| `file` | File storage abstraction/providers | Keep/adapt |
| `notification` | Notification channel/provider abstraction | Keep/adapt |
| `translation` | Localized resource content | Keep/adapt |
| `search` | Search abstraction/indexing | Keep/adapt |
| `settings` | Operational/system settings | Keep/adapt |
| `analytics` | Event/metric analytics integration | Keep/adapt |
| `api-key` | Integration/API credentials where required | Keep/adapt |

### 5.2 Rehla-specific modules

| Module | Responsibility |
|---|---|
| `application` | Visa application lifecycle and applicant/business data |
| `documents` | Applicant document requirements, submissions, review state |
| `tracking` | Application status history and customer-visible progress |

These are first-class Rehla modules because they are core business concepts and have their own data, services, validation, and tests.

### 5.3 Application/content feature

`banners` are **not** a core infrastructure module equivalent to Product, Payment, or Customer.

They belong under application/content composition, for example:

```text
apps/api/src/features/content/banners/
```

The Admin and Web applications consume banner APIs and render banner resources/placements. This keeps content composition separate from the generic module layer.

---

## 6. Medusa Keep / Adapt / Exclude Boundary

### Keep or adapt

```text
Auth
User
RBAC
Customer
Store
Product
Pricing
Currency
Region
Cart
Payment
File
Notification
Translation
Search
Settings
Analytics
API Key
Event Bus
Workflow Engine
Caching
Locking
Link Modules
```

These boundaries provide capabilities that have a direct or infrastructure-level relationship with Rehla.

### Rehla-specific replacement/extension

```text
Application
Documents
Tracking
Bank Transfer Payment Provider
Banner/Content feature
```

### Exclude from the current baseline

```text
Fulfillment
Inventory
Stock Location
Shipping-specific order flows
Travel-agency marketplace
Flight booking
Hotel booking
Physical-goods logistics
```

`Order`, `Promotion`, `Sales Channel`, and `Tax` should not be introduced solely because they exist in Medusa. They belong only when an explicit Rehla requirement establishes their need.

---

## 7. Domain Relationship Model

The primary Rehla runtime relationship is:

```text
Store
├── Customer
│   └── Cart
│       └── Line Item
│           └── Product = Visa Service
│               └── Application
│                   ├── Documents → File
│                   ├── Payment
│                   └── Tracking
│                           └── Notification
└── Admin User
    └── RBAC

Banner
└── Application/Content Feature
```

This model deliberately avoids creating a separate `Visa` commerce module. The visa offering is a Product/commerce resource; the visa application is a separate Rehla domain object created from the purchased service.

---

## 8. Module Internal Structure

A Rehla business module should follow the same useful internal separation visible in the supplied Medusa custom-module and module packages:

```text
packages/modules/<module>/
├── src/
│   ├── models/
│   ├── services/
│   ├── workflows/              # only when module-local workflows are appropriate
│   ├── types/
│   ├── index.ts
│   └── ...
├── migrations/
├── integration-tests/
├── unit-tests/
├── package.json
└── README.md
```

The exact files vary by module. A module owns its persistence models, migration history, service contract, validation/types, and module-level tests.

---

## 9. Cross-Module Communication

Direct coupling to another module's internal implementation is not the default integration mechanism.

### 9.1 Services

Use resolved module services for operations owned by another module when a direct request/response interaction is required.

### 9.2 Links

Use explicit links for relationships between entities owned by different modules. Examples relevant to Rehla include:

```text
Product ↔ Pricing
Customer ↔ Payment
Cart ↔ Product
Cart ↔ Customer
Application ↔ Documents
Application ↔ Payment
Application ↔ Tracking
Tracking ↔ Notification
```

The final relationships should be encoded as Rehla links rather than copied foreign persistence logic across modules.

### 9.3 Events

Use events for asynchronous reactions and integration side effects.

Examples:

```text
application.created
application.submitted
application.status_changed
payment.submitted
payment.verified
payment.rejected
tracking.updated
notification.requested
```

Event names and payloads must remain part of an explicit contract.

### 9.4 Workflows

Use workflows for multi-step business operations crossing module boundaries, especially operations that need retries, compensation, idempotency, or explicit step boundaries.

Examples:

```text
Create Visa Application
Submit Visa Application
Submit Bank Transfer Receipt
Verify Payment
Change Application Status
Notify Customer of Status Change
```

---

## 10. Payment Architecture

Payment remains a provider-based abstraction rather than hard-coding bank-transfer logic into the generic payment module.

```text
Payment Module
      │
      ├── payment contract
      │
      └── Rehla Bank Transfer Provider
              ├── transfer instructions
              ├── receipt file reference
              ├── submission state
              └── admin verification/rejection
```

Stripe is not part of the current Rehla baseline because bank transfer is the stated payment method.

The uploaded receipt is stored through the File abstraction. Payment verification is an operational state transition and should be auditable.

---

## 11. Application Lifecycle

The application module owns the business lifecycle of a visa request.

A canonical flow is:

```text
cart
  ↓
application draft
  ↓
application submitted
  ↓
documents submitted / reviewed
  ↓
payment submitted
  ↓
payment verified / rejected
  ↓
application processing
  ↓
status changes
  ↓
completed / rejected / cancelled
```

The exact state machine and valid transitions belong to the Application module and must be enforced at the service/workflow layer, not only in the Admin UI.

---

## 12. Documents and File Storage

The architecture separates:

```text
Document = business/domain record
File     = storage abstraction
```

For example, a document record can contain the document type, applicant/application relation, review state, and file reference. Physical storage details are delegated to the File module/provider.

This allows storage providers to change without changing the application-domain model.

---

## 13. Tracking and Notifications

Tracking is a Rehla domain module because application progress is customer-visible business data.

```text
Application
   ↓
Tracking entry / status transition
   ↓
Event
   ↓
Notification
   ↓
Customer channel
```

The Notification module remains channel/provider-oriented; Tracking owns the business meaning of the status change.

---

## 14. Admin Architecture

The Admin is split conceptually into two layers.

### Shared/Admin infrastructure

```text
admin shell
routing
permissions
query/data access
shared loaders/hooks
shared layout primitives
UI components
icons
SDK integration
```

### Domain resources

```text
Dashboard
Customers
Visa Services (Products)
Applications
Documents
Payments
Tracking
Banners / Content
Admin Users
Roles / Permissions
Settings
Reports / Analytics
```

Domain resources stay in `apps/admin`; generic visual primitives remain in `packages/ui`.

This preserves the important Medusa distinction between shared Admin infrastructure and business resources.

---

## 15. Dashboard Responsibility

The dashboard is an application surface, not a domain module.

Its role is to aggregate operational information such as:

```text
pending applications
pending payment reviews
recent applications
status distribution
customer activity
key business metrics
content/banner state
```

Dashboard queries should use established API/application contracts rather than reading module-private database structures.

---

## 16. Dependency Direction

The preferred dependency direction is:

```text
apps/*
  ↓
API contracts / SDK
  ↓
application composition
  ↓
modules
  ↓
persistence/infrastructure providers
```

A domain module must not depend on an Admin page or a Web component. UI packages must not own business-domain persistence.

Infrastructure modules such as Event Bus, Caching, Locking, and Workflow Engine support domain execution without becoming the domain itself.

---

## 17. Data Ownership Rules

Each module owns its own domain records and migrations.

Examples:

```text
Product       → product module
Customer      → customer module
Application   → application module
Document      → documents module
Payment       → payment module
Tracking      → tracking module
Notification  → notification module
```

Application code may coordinate reads and writes through explicit contracts, but cross-module duplication of ownership is prohibited.

---

## 18. Infrastructure Modules

The following modules are architectural infrastructure rather than customer-facing domains:

```text
Event Bus
Workflow Engine
Caching
Locking
Link Modules
Search
Analytics
API Key
```

They provide execution and integration mechanisms for the application.

For distributed deployments, the Redis-backed providers already represented in the supplied Medusa source are the relevant provider pattern; in-memory variants are development/test alternatives rather than the production architecture baseline.

---

## 19. Search Architecture

Search is a distinct application capability rather than a property embedded directly into every domain module.

The search layer is composed from explicit index definitions and module/application data. Visa services should be searchable by the business fields required by Rehla, while applications/payments/search access remain controlled by the appropriate permissions and contracts.

---

## 20. Localization

Rehla requires Arabic and English as the current supported languages.

Localization is separated from domain persistence so translated text can be handled consistently across Admin and Web surfaces.

The content model should distinguish:

```text
canonical domain data
localized presentation data
```

RTL behavior belongs to the Web/Admin UI layer, while translation data/contracts belong to the Translation capability.

---

## 21. Security Boundaries

Security enforcement must exist at multiple layers:

```text
Authentication
   ↓
Authorization / RBAC
   ↓
API validation
   ↓
Service/workflow invariants
   ↓
Data ownership
```

Admin permission checks are not a replacement for backend authorization. Sensitive operations such as payment verification, document review, and application status transitions must be protected server-side.

---

## 22. Testing Architecture

The repository uses multiple test levels:

```text
unit tests
   ↓
integration tests
   ↓
HTTP/API tests
   ↓
Admin/Web component tests
   ↓
critical-path E2E
```

Modules should be testable independently where practical. Cross-module workflows require integration coverage. High-value customer/admin journeys require end-to-end coverage.

---

## 23. Deployment Architecture

The logical architecture remains a modular monolith even when deployed across multiple runtime processes or supporting services.

```text
                   ┌───────────────┐
                   │      Web      │
                   └───────┬───────┘
                           │
                   ┌───────▼───────┐
                   │   Rehla API   │
                   │ modular monolith│
                   └───────┬───────┘
                           │
      ┌────────────────────┼────────────────────┐
      │                    │                    │
┌─────▼─────┐       ┌──────▼──────┐      ┌──────▼──────┐
│ PostgreSQL│       │    Redis    │      │ File/Object │
│   DB      │       │ cache/locks │      │   Storage   │
└───────────┘       └─────────────┘      └─────────────┘

                   ┌───────────────┐
                   │     Admin     │
                   └───────┬───────┘
                           │
                           └──────→ Rehla API
```

The exact infrastructure topology is defined in the infrastructure and production-release plans, not in individual business modules.

---

## 24. Architecture Decision Summary

| Decision | Rehla choice |
|---|---|
| Architecture | Modular monolith |
| Ownership | Rehla-owned monorepo |
| Medusa relationship | Selective compatibility/pattern reuse, not a fork/copy |
| Backend | TypeScript/Node modular application |
| Admin | React/Vite-like Rehla Admin inspired by Medusa Dashboard patterns |
| Web | Next.js customer storefront |
| Commerce product | Product represents a Visa Service |
| Visa application | Separate `application` domain module |
| Documents | Separate `documents` module + File abstraction |
| Payment | Generic Payment module + Bank Transfer provider |
| Tracking | Separate Rehla module |
| Banners | App/content feature, not a core module package |
| UI package | Generic design system only |
| Marketplace | Excluded |
| Flights | Excluded |
| Hotels | Excluded |
| Shipping/Fulfillment | Excluded |

---

## 25. Dependency Graph

```text
Foundation
    ↓
Architecture
    ↓
Medusa compatibility boundary
    ↓
Auth
    ↓
Store + Customer + User + RBAC
    ↓
Product / Visa
    ↓
Pricing + Currency + Region
    ↓
Cart
    ↓
Application
    ↓
Documents + File
    ↓
Payment + Bank Transfer
    ↓
Tracking + Notifications
    ↓
Banners + Content
    ↓
Search + Translation + Settings
    ↓
Workflows + Events + Jobs + Links
    ↓
Admin Shell
    ↓
Admin Resources
    ↓
Web Storefront
    ↓
Testing + Security
    ↓
Observability + Analytics
    ↓
Infrastructure + Staging
    ↓
Production Release
```

This ordering is the implementation dependency sequence used by the accompanying roadmap; individual technical dependencies may allow some work to proceed in parallel once their contracts are stable.

---

## 26. Source Basis

This architecture is grounded in the supplied project-planning documents and the supplied Medusa source snapshot, especially the observable separation between:

- reusable modules
- framework/application composition
- Admin infrastructure and Admin resources
- design-system packages
- workflows, subscribers, jobs, links, and loaders
- provider-based infrastructure
- custom-module structure and registration

Rehla-specific decisions in this document are limited to the established Rehla scope and decisions recorded for this project.
