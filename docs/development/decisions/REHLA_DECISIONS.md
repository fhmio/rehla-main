# Rehla — Complete Product & Engineering Decisions

<div align="center">

# رحله · Rehla

**The standalone decision record for the Rehla platform**

`PRODUCT` · `DOMAIN` · `USER FLOWS` · `ADMIN FLOWS` · `ARCHITECTURE` · `SECURITY` · `OPERATIONS`

**Status:** Current Baseline  
**Audience:** Backend · Admin · Web · QA · DevOps · AI Coding Agents  
**Document role:** Self-contained source of truth for project decisions

</div>

---

## 0. How This Document Must Be Used

This document is intentionally **standalone**.

A developer, reviewer, QA engineer, DevOps engineer, or AI coding agent should be able to read this file alone and understand the current Rehla product and engineering decisions without having to consult another project document.

### Decision precedence

When implementing Rehla, follow this order:

1. **Explicit decisions in this document** are authoritative.
2. **Business/domain boundaries** in this document are stricter than generic framework conventions.
3. **Implementation details must preserve the behavior and boundaries defined here.**
4. A technology, framework, package, or pattern may be changed later only through an explicit decision update; it must not be changed implicitly during implementation.

### What this document controls

This document defines:

- what Rehla is;
- what Rehla sells;
- who uses it;
- what customers can do;
- what Admin/staff can do;
- how the application lifecycle works;
- how payment works;
- how documents work;
- what each domain module owns;
- how the Admin, Web, and API relate to the domain;
- which Medusa ideas may be reused;
- which Medusa domains must remain outside Rehla;
- repository and architectural boundaries;
- authorization and security expectations;
- testing and quality expectations;
- deployment principles;
- the distinction between CRUD data management and domain/workflow behavior;
- explicit non-decisions and historical decisions that must not be accidentally revived.

---

# 1. Product Identity

## 1.1 Product name

**Rehla / رحله**

## 1.2 Product category

A **visa-service commerce and visa-application operations platform**.

## 1.3 Primary audience

Sudanese travelers seeking visa services, including Umrah and tourist visas for Gulf destinations.

## 1.4 Product promise

Rehla provides a controlled digital flow from:

```text
Visa Service Discovery
        ↓
Service Selection
        ↓
Customer Account
        ↓
Application
        ↓
Applicant Documents
        ↓
Bank Transfer
        ↓
Receipt Submission
        ↓
Operational Review
        ↓
Application Processing
        ↓
Tracking
        ↓
Completion
```

## 1.5 Core business distinction

This distinction is mandatory throughout the platform:

```text
Product = Visa Service offered for sale
Application = Customer's actual request for that Visa Service
```

A **Visa** is not a second independent commerce catalog concept when the existing Product model already represents the sellable service.

---

# 2. Current Scope

## 2.1 Included capabilities

| Area | Decision |
|---|---|
| Visa Services | Rehla sells visa services as Products |
| Umrah | Included |
| Gulf tourist visas | Included |
| Customer accounts | Included |
| Customer authentication | Included |
| Service catalog | Included |
| Service cart | Included |
| Visa applications | Included |
| Applicant information | Included |
| Required documents | Included |
| File storage | Included |
| Bank-transfer payment | Included |
| Payment receipt upload | Included |
| Manual payment verification | Included |
| Application tracking | Included |
| Status history | Included |
| Notifications | Included |
| Banners/content | Included |
| Admin dashboard | Included |
| Admin users | Included |
| RBAC | Included |
| Settings | Included |
| Search | Included |
| Translation | Included |
| Reporting/analytics | Included |
| API contracts | Included |
| Background jobs/events/workflows | Included |

## 2.2 Explicit exclusions

The following are **outside the current Rehla baseline** and must not be introduced by accidental reuse:

- flight booking;
- hotel booking;
- travel-agency marketplace functionality;
- multi-vendor agency offers/catalogs;
- shipping;
- fulfillment logistics;
- physical-goods inventory;
- stock-location operations used for physical commerce;
- generic warehouse flows;
- loyalty as a core commerce domain;
- shipping-method checkout;
- physical delivery tracking;
- unrelated general-purpose commerce features.

These are **architectural exclusions**, not simply deferred UI screens.

---

# 3. Actors and Responsibilities

## 3.1 Customer

The customer is the primary external actor.

### Customer responsibilities

The customer can:

- discover published Visa Services;
- inspect service details and pricing;
- add a Visa Service to the cart;
- create/sign into a customer account;
- create or continue an application;
- provide applicant information;
- upload required documents;
- submit the application;
- view payment instructions;
- complete the bank transfer outside Rehla's online gateway layer;
- upload payment evidence/receipt;
- view payment review state;
- view application status;
- view tracking history;
- receive notifications;
- manage customer profile information.

The customer cannot:

- change server-authoritative prices;
- approve their own payment;
- approve their own documents;
- force an application status transition;
- bypass required documents;
- access another customer's application;
- access Admin resources.

## 3.2 Admin / Staff

Admin is an **operational application**, not a second business domain.

Admin staff can operate the resources allowed by their permissions, including:

- dashboard/overview;
- customers;
- Visa Services;
- applications;
- applicant documents;
- payments;
- tracking/statuses;
- banners/content;
- Admin users;
- roles and permissions;
- settings;
- reports/analytics.

## 3.3 System / background runtime

The runtime performs technical and asynchronous responsibilities such as:

- workflows;
- event publication and consumption;
- scheduled jobs;
- notification dispatch;
- search indexing;
- caching;
- distributed locking;
- file provider interaction;
- analytics integration.

The runtime does **not** replace human decisions that are explicitly operational, especially:

- payment verification;
- document review;
- sensitive application-state decisions.

---

# 4. Customer Experience Decisions

## 4.1 Customer journey — complete flow

```text
┌──────────────────────┐
│ Discover Rehla       │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Browse Visa Services │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Service Detail       │
│ price / destination  │
│ requirements / info  │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Add to Service Cart  │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Customer Account     │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Start Application    │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Applicant Data       │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Required Documents   │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Review / Submit      │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Bank Transfer        │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Receipt Upload       │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Admin Review         │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Processing / Status  │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Tracking + Alerts    │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Approved / Completed │
└──────────────────────┘
```

## 4.2 Discovery

The storefront exposes published Visa Services and supporting content such as banners.

A customer can navigate from:

```text
Home
  ↓
Banner / category / service listing
  ↓
Visa Service detail
```

A banner is content and navigation surface, not a core commerce module.

A banner may be configured with a destination/action that opens an appropriate Rehla destination such as a service detail or other supported content route.

## 4.3 Visa Service detail

A Visa Service/Product is the sellable offer.

The service detail surface can present:

- service title;
- service description;
- destination/country context;
- service category;
- available price/currency context;
- service-specific requirements;
- other business-defined descriptive information;
- call-to-action to add the service to the cart/start the application flow.

## 4.4 Cart

The cart is a **service cart**, not a physical-commerce shipping cart.

The cart is responsible for the commercial selection of Visa Service Products and relevant pricing/totals.

Shipping-specific concepts are deliberately not part of the baseline cart model:

- shipping address;
- shipping method;
- shipping option pricing;
- fulfillment shipping state;
- delivery logistics.

The cart must not silently inherit such capabilities from Medusa-style commerce code.

## 4.5 Application creation

An application is created for a selected Visa Service.

The application is a separate domain object because the commercial product and the operational visa request have different lifecycles.

The application may contain:

- customer relationship;
- Visa Service reference;
- applicant information;
- price/currency snapshot;
- application status;
- document requirements/submissions;
- payment relationship;
- tracking/status history;
- audit-relevant operational metadata.

## 4.6 Price and currency authority

The server is authoritative for the price and currency associated with the application.

A client must not be able to override the price through a request payload.

Where multiple currencies are supported, the selected price/currency context is part of the application/commercial snapshot so historical applications do not change simply because the current Visa Service is edited later.

The established currency context includes **SDG** and **SAR** support.

Automatic exchange-rate conversion is not a substitute for explicit service pricing decisions.

## 4.7 Applicant information

Applicant data is part of the application workflow, not part of the generic Product definition.

The Product defines what service is sold and what requirements are relevant.

The Application stores the customer's actual submitted values.

## 4.8 Documents

Documents are required application evidence and are managed independently from physical file storage.

The conceptual separation is:

```text
Document = business record
File     = physical/object-storage representation
```

This means a document can have:

- document type/requirement identity;
- application relation;
- applicant relation where required;
- file reference;
- submission/review state;
- review metadata;
- version/history information where re-submission is supported.

## 4.9 Payment

The baseline payment method is **bank transfer**.

The customer workflow is:

```text
Application
   ↓
Payment instructions
   ↓
Customer performs bank transfer
   ↓
Customer uploads receipt/evidence
   ↓
Payment enters review
   ↓
Admin verifies or rejects
```

The application/payment process must not depend on an online card gateway for the baseline experience.

## 4.10 Receipt behavior

A payment receipt is evidence submitted for review.

The receipt is a private file/resource and must not become publicly addressable merely because it is stored in object storage.

A rejected receipt may be replaced by a new submission while previous evidence remains preserved for audit/history purposes.

## 4.11 Tracking

Tracking is a first-class Rehla business concern.

The customer can inspect:

- current application status;
- previous status history;
- operational progress;
- important updates/messages exposed through the tracking experience.

Status history must be immutable from the customer-facing side.

Every accepted application-status transition produces a timeline/history record containing at minimum:

- previous state;
- new state;
- actor/context;
- timestamp;
- optional operational note.

## 4.12 Notifications

Important operational changes can generate customer notifications.

Typical notification triggers include:

- application created/submitted;
- payment receipt submitted;
- payment verified;
- payment rejected;
- application status changed;
- additional document requested;
- application completed.

Notification delivery is separated from the core application state machine.

---

# 5. Admin Experience Decisions

## 5.1 Admin purpose

The Admin exists to operate Rehla.

It is not merely a CRUD interface and it must not contain the primary domain business logic.

The Admin is responsible for:

```text
Display
Input
Filters
Actions
Permissions
Navigation
Operational UX
```

The API/modules remain responsible for:

```text
Validation
Authorization enforcement
Business rules
State transitions
Persistence
Workflows
Transactions/idempotency
Audit-sensitive operations
```

## 5.2 Admin navigation model

The current Admin resource surface is:

```text
Dashboard
├── Customers
├── Visa Services
├── Applications
├── Documents
├── Payments
├── Tracking
├── Banners / Content
├── Admin Users
├── Roles / Permissions
├── Settings
└── Reports / Analytics
```

## 5.3 Admin dashboard

The dashboard provides operational visibility rather than generic e-commerce metrics only.

Relevant operational areas include:

- application pipeline;
- payment review workload;
- document review workload;
- status distribution;
- recent applications;
- recent payment submissions;
- operational exceptions/attention areas;
- service/catalog activity;
- reporting/analytics surfaces.

## 5.4 Application management

Application detail is the central operational workspace.

An Admin should be able to inspect, according to permissions:

```text
Application
├── Customer
├── Visa Service snapshot
├── Applicant data
├── Documents
├── Payment
├── Current status
├── Status timeline
└── Operational actions
```

## 5.5 Payment management

Payment review is an explicit Admin resource.

An authorized Admin can:

- inspect payment submission data;
- inspect receipt/evidence securely;
- verify payment;
- reject payment;
- record operational context/note where supported.

The UI must never rely on a hidden client-side flag to establish that payment is verified. The backend remains authoritative.

## 5.6 Document management

The Admin can review application documents and their review status according to permissions.

Document review state must be enforced on the backend.

## 5.7 Tracking management

Authorized Admin users can update application progress through the approved state machine.

Tracking updates should create history and may trigger notifications.

## 5.8 Visa Service management

The Admin manages the catalog of sellable Visa Services.

Typical operations include:

- create;
- edit;
- publish/unpublish;
- organize by category/destination context;
- configure pricing;
- define service information;
- define service requirements where the Product/requirement model supports them.

## 5.9 Banner/content management

Banners are managed as application/content functionality.

They are not a first-class reusable domain module under `packages/modules` merely because they appear in the Admin navigation.

## 5.10 Admin Users and RBAC

Staff identities are separate from customer identity.

The security model is:

```text
Admin User
    ↓
Role(s)
    ↓
Permission(s)
    ↓
Protected Admin resource/action
```

Frontend permission guards improve UX but are never the final authorization boundary.

---

# 6. Application Lifecycle Decisions

## 6.1 Canonical application lifecycle

The established canonical baseline is:

```text
DRAFT
  ↓
DOCUMENTS_REQUIRED
  ↓
AWAITING_PAYMENT
  ↓
PAYMENT_REVIEW
  ↓
SUBMITTED
  ↓
UNDER_REVIEW
  ↓
PROCESSING
  ↓
APPROVED
  ↓
COMPLETED
```

Operational rejection outcomes are permitted from the appropriate processing/review stages, subject to the server-enforced transition rules.

## 6.2 Lifecycle rules

1. Customers may create/continue their own application context.
2. Required data/documents must be validated before submission.
3. Payment verification is an Admin-controlled operation.
4. State changes must occur through domain/service/workflow logic.
5. The Admin UI cannot manufacture a legal state transition.
6. Invalid transitions must be rejected by the backend.
7. Accepted transitions are recorded in status history.
8. Status history is not silently rewritten when the current state changes.
9. State changes that require communication can emit events consumed by notification logic.

## 6.3 Rejection model

Rejection is an operational outcome, not a bypass around validation.

For example:

```text
UNDER_REVIEW
     ↓
REJECTED
```

or

```text
PROCESSING
     ↓
REJECTED
```

Exact rejection reasons should be represented explicitly when they are required for operations, auditability, or customer communication.

## 6.4 Completion

`COMPLETED` means the Rehla-defined processing lifecycle for the application has reached its terminal successful state.

The application should remain historically consistent after completion.

---

# 7. Document Lifecycle Decisions

## 7.1 Document review states

The document baseline uses:

```text
PENDING
ACCEPTED
REJECTED
```

## 7.2 Re-submission

When an Admin requests a replacement document, a new submission may be stored without destroying the prior submission history.

This supports:

```text
Document v1 → REJECTED
Document v2 → PENDING
Document v2 → ACCEPTED
```

## 7.3 Security

Document access must be authorized server-side.

Storage keys are generated/controlled by the backend rather than accepted blindly from a client.

The backend must validate at minimum:

- resource ownership/context;
- allowed file type/MIME policy;
- file size policy;
- safe/generated storage key;
- authorization to read/review/download.

---

# 8. Payment Decisions

## 8.1 Payment model

Rehla uses a provider-based Payment abstraction, with a **Bank Transfer provider** as the baseline implementation.

Conceptually:

```text
Payment Module
      │
      └── Bank Transfer Provider
              ├── transfer instructions
              ├── submission reference/evidence
              ├── review state
              └── Admin verification/rejection
```

## 8.2 Payment boundaries

The generic Payment module owns payment abstractions/contracts.

The bank-transfer provider owns bank-transfer-specific behavior.

The Application module owns application lifecycle semantics.

The File module owns file/storage behavior.

This prevents bank-transfer details from being hard-wired into every domain.

## 8.3 Server-authoritative pricing

The application/payment amount is derived server-side from the commercial/application snapshot.

A client cannot submit an arbitrary amount and have the backend accept it as the official application price.

## 8.4 Receipt review

Payment receipt submission is not payment verification.

```text
Receipt uploaded
    ≠
Payment verified
```

Verification requires an authorized server-side operational action.

## 8.5 Online gateways

Online card/payment gateways are not part of the current baseline.

A future provider can be added behind the Payment abstraction without changing the core application model, but doing so requires an explicit project decision update.

---

# 9. Visa Service / Product Decisions

## 9.1 Product semantics

The Product domain represents sellable Visa Services.

Examples conceptually include:

- Umrah visa service;
- Gulf tourist visa service;
- other approved visa-service offerings.

## 9.2 Product categories

Product categories may provide hierarchical organization for the catalog where useful.

A category can represent a domain grouping such as visa type or service family without creating another duplicate `Visa` commerce entity.

## 9.3 Destination/country context

Country/destination is a reusable platform concept and may be referenced by the relevant service context.

## 9.4 Requirements

Service-specific requirements belong to the service/application requirement boundary.

The Product definition determines what requirements are necessary; the Application stores what the customer actually submitted.

## 9.5 Historical integrity

Once an application has been created from a service, later edits to the current Product must not rewrite the historical commercial/application data that was actually used by that application.

Relevant information should therefore be snapshotted where historical correctness requires it, especially:

- product/service identity;
- price;
- currency;
- relevant service/requirement definition context.

---

# 10. Cart Decisions

## 10.1 Purpose

The Cart is a commercial selection boundary for Visa Service Products.

## 10.2 What the Cart keeps

The cart may retain the useful commerce concepts needed by Rehla, such as:

- cart identity;
- customer relation;
- currency context;
- line items;
- product references;
- pricing/totals;
- line-item adjustments where required.

## 10.3 What the Cart deliberately does not inherit

The Rehla cart does not inherit generic physical-commerce behavior for:

- shipping address;
- billing/shipping address workflows where not required by Rehla;
- shipping methods;
- delivery fees;
- fulfillment state;
- warehouse logistics;
- physical delivery tracking.

## 10.4 Single-purpose principle

A cart feature is justified by an actual Rehla business behavior. Medusa-style commerce code must not be imported just because it exists upstream.

---

# 11. Customer and Identity Decisions

## 11.1 Customer vs Admin identity

These are different security contexts:

```text
Customer Identity
    ↓
Customer-owned data and storefront capabilities

Admin Identity
    ↓
Staff role(s)
    ↓
Permissions
    ↓
Operational resources
```

## 11.2 Ownership boundary

Customer-facing application/document/payment resources must be owner-scoped.

A customer request must never obtain another customer's resource merely by changing an identifier in a URL or request body.

## 11.3 Admin authorization

Admin authorization is enforced on the backend.

A frontend-only permission guard is insufficient.

Sensitive actions include at minimum:

- payment verification/rejection;
- document review;
- application status transitions;
- Admin user management;
- role/permission management;
- protected settings changes.

---

# 12. Module Decisions

## 12.1 Core module map

| Module | Ownership / responsibility | Rehla status |
|---|---|---|
| `store` | Store/commerce context | Keep / adapt |
| `customer` | Customer identity/profile | Keep / adapt |
| `user` | Admin/staff identity | Keep / adapt |
| `auth` | Authentication/provider boundary | Keep / adapt |
| `rbac` | Roles, permissions, policies | Keep / adapt |
| `product` | Visa Service catalog | Keep / adapt |
| `pricing` | Prices and pricing behavior | Keep / adapt |
| `currency` | Currency definitions/context | Keep / adapt |
| `region` | Destination/country/market context | Adapt |
| `cart` | Visa Service cart | Adapt |
| `payment` | Payment abstraction/provider boundary | Adapt |
| `file` | Storage abstraction/providers | Keep / adapt |
| `notification` | Notification channels/providers | Keep / adapt |
| `translation` | Localized content/resources | Keep / adapt |
| `search` | Search abstraction/indexing | Keep / adapt |
| `settings` | System/operational configuration | Keep / adapt |
| `analytics` | Analytics/event integration | Keep / adapt |
| `api-key` | API/integration credentials where required | Keep / adapt |
| `event-bus-*` | Event transport | Infrastructure |
| `workflow-engine-*` | Workflow execution | Infrastructure |
| `locking` | Concurrency/distributed locks | Infrastructure |
| `caching` | Cache abstraction | Infrastructure |
| `link-modules` | Cross-module relationships | Infrastructure |

## 12.2 Rehla-specific modules

These are first-class business modules because they represent core Rehla behavior:

| Module | Responsibility |
|---|---|
| `application` | Visa application lifecycle and applicant/business data |
| `documents` | Requirements, submissions, review, history |
| `tracking` | Application progress and status history |

## 12.3 Banner decision

There is **no `packages/modules/banner` core domain module** in the Rehla baseline.

Banners belong to an application/content feature boundary because their responsibility is storefront content and navigation presentation rather than a reusable commerce domain entity.

Recommended ownership boundary inside the application:

```text
apps/api/src/features/content/banners
```

The Admin and Web may expose banner resources/surfaces, but that does not make Banner a core platform module.

## 12.4 Generic UI decision

`@rehla/ui` or equivalent shared UI package is a **generic design system only**.

It may contain:

- buttons;
- inputs;
- dialogs;
- tables/primitives;
- typography;
- layout primitives;
- generic form controls;
- generic navigation primitives;
- accessibility utilities;
- visual tokens.

It must not become the owner of business resources such as:

- Visa;
- Application;
- Document;
- Payment;
- Tracking;
- Customer;
- Admin User.

Those remain Admin resources/features inside `apps/admin`.

---

# 13. Cross-Module Communication Decisions

## 13.1 Explicit contracts

Modules own their internal implementation and expose stable public contracts.

Modules should not reach into another module's private files, tables, or implementation details as a normal integration mechanism.

## 13.2 Direct service interaction

Use resolved module services/contracts when a synchronous operation is required from another module.

Example:

```text
Application
   ↓
request Document operations through public Document contract
```

not:

```text
Application
   ↓
import Documents private repository/model internals
```

## 13.3 Links

Use explicit links to represent relationships between entities owned by different modules.

Relevant relationships include:

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

## 13.4 Events

Events are for decoupled side effects and asynchronous reactions.

Relevant event concepts include:

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

Event contracts must be explicit and version-conscious.

## 13.5 Workflows

Workflows coordinate multi-step business operations crossing module boundaries.

Examples:

```text
Create Visa Application
Submit Visa Application
Submit Payment Receipt
Verify Payment
Reject Payment
Change Application Status
Notify Customer of Status Change
```

Workflows are appropriate where the operation benefits from:

- explicit steps;
- retries;
- idempotency;
- compensation;
- cross-module coordination.

---

# 14. API Decisions

## 14.1 API boundary

The API is the application-facing contract for Web and Admin.

```text
Web ───┐
       ├──→ Rehla API ───→ Modules / Workflows / Services
Admin ─┘
```

The Web and Admin should not bypass the API and access another application's database directly.

## 14.2 API responsibilities

The API layer is responsible for:

- transport;
- request/response validation;
- authentication context;
- authorization enforcement;
- orchestration entry points;
- mapping requests into domain services/workflows;
- consistent API error contracts;
- exposing the supported public contract.

## 14.3 Domain ownership

The API application must not become a giant persistence layer.

Business entities remain owned by their modules.

---

# 15. Admin Architecture Decisions

## 15.1 Admin technology boundary

Admin is a dedicated **React/Vite-like operational application** inspired by useful patterns observed in the Medusa Dashboard.

The decision is about the architectural role and behavior, not about making Rehla a copy of Medusa Dashboard source.

## 15.2 Useful Admin patterns to retain

The Admin may use proven interaction patterns such as:

- route-based resource screens;
- list/detail resource structures;
- query/data hooks;
- configurable tables;
- filters;
- actions;
- permission guards;
- layouts;
- extension/resource zones where required.

## 15.3 Admin package boundaries

The Admin ecosystem may contain concepts such as:

```text
apps/admin
├── dashboard shell
├── resource pages
├── route definitions
├── data/query layer
├── forms
├── permission guards
└── reusable UI from @rehla/ui
```

A Medusa-style split such as `admin-bundler`, `admin-sdk`, `admin-shared`, and `admin-vite-plugin` is an implementation pattern to evaluate/use where it materially fits Rehla; it is not an automatic requirement to reproduce Medusa's package tree.

---

# 16. Web Decisions

## 16.1 Web role

`apps/web` is the customer-facing storefront.

## 16.2 Web responsibilities

The Web exposes the customer experience for:

- Home/content;
- Visa Service discovery;
- Visa Service details;
- cart;
- account;
- applications;
- document submission;
- payment instructions/receipt submission;
- tracking;
- notifications;
- profile/settings where applicable.

## 16.3 Web technology

The established architecture uses a **Next.js customer storefront**.

Next.js is the storefront application boundary; it does not own the business domain or replace the API.

---

# 17. Backend Architecture Decisions

## 17.1 Architectural style

Rehla is a **modular monolith**.

It is not a microservices system.

## 17.2 Ownership

Rehla owns the repository and architecture.

Medusa is a reference source for proven architecture/patterns and compatible building blocks, not the owner of Rehla.

## 17.3 Backend technology boundary

The current backend decision is a **TypeScript/Node modular application**.

The critical architectural decision is modular ownership and contracts; a framework must not erase those boundaries.

## 17.4 Application composition

The API application composes:

```text
Modules
Workflows
Subscribers
Jobs
Links
Search
Loaders / startup composition
```

The application composition layer coordinates; domain modules own their business state.

---

# 18. Repository Decisions

## 18.1 Monorepo

Rehla is an independent monorepo.

The repository is organized around applications and packages rather than multiple disconnected repositories that duplicate shared domain contracts.

## 18.2 Target repository shape

```text
rehla/
├── apps/
│   ├── api/
│   ├── admin/
│   └── web/
│
├── packages/
│   ├── modules/
│   │   ├── store/
│   │   ├── customer/
│   │   ├── user/
│   │   ├── auth/
│   │   ├── rbac/
│   │   ├── product/
│   │   ├── pricing/
│   │   ├── currency/
│   │   ├── region/
│   │   ├── cart/
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
│   │   ├── locking/
│   │   ├── caching/
│   │   ├── link-modules/
│   │   ├── event-bus-*/
│   │   └── workflow-engine-*/
│   │
│   ├── ui/
│   ├── icons/
│   ├── sdk/
│   ├── contracts/
│   └── config/
│
├── integration-tests/
├── docs/
└── scripts/
```

## 18.3 Module internal structure

A typical domain package follows the conceptual boundary:

```text
packages/modules/<module>/
├── src/
│   ├── models/
│   ├── services/
│   ├── types/
│   ├── index.ts
│   └── ...
├── migrations/
├── integration-tests/
├── unit-tests/
├── package.json
└── README.md
```

Exact file layout can vary as long as module ownership and public contracts remain explicit.

---

# 19. Medusa Reuse Decisions

## 19.1 Fundamental rule

Rehla may reuse **architecture, patterns, compatible contracts, UI ideas, or selectively compatible code**, but Rehla must not become a Medusa fork or a second copy of Medusa.

## 19.2 What is retained/adapted

Useful Medusa concepts for Rehla include:

- modular domain services;
- provider abstractions;
- workflows;
- events/subscribers;
- jobs;
- explicit links;
- application loaders/composition;
- Admin resource architecture;
- configurable data tables;
- permission-aware Admin actions;
- SDK/API conventions;
- generic design-system patterns;
- file provider abstraction;
- notification provider abstraction;
- payment provider abstraction;
- search abstraction;
- caching/locking infrastructure.

## 19.3 What is not copied

Rehla must not copy an unrelated Medusa domain simply because it is available upstream.

In particular, the following Medusa-oriented domains are outside the Rehla baseline unless a future decision explicitly reintroduces them:

- fulfillment;
- inventory;
- stock locations;
- shipping operations;
- shipping methods;
- physical warehouse logic;
- generic order-domain complexity that duplicates Application;
- marketplace vendor/agency behavior;
- unrelated promotions/tax/commercial subsystems.

## 19.4 Product adaptation

Medusa's Product concept is retained because it maps cleanly:

```text
Medusa Product concept
        ↓ adaptation
Rehla Product
        ↓
Visa Service
```

The application is a separate Rehla-specific domain.

## 19.5 Order decision

A generic Medusa Order subsystem is **not** the center of Rehla's workflow.

Rehla's business process centers on:

```text
Cart
  ↓
Application
  ↓
Documents
  ↓
Payment
  ↓
Tracking
```

This avoids recreating shipping/order/fulfillment complexity that is not part of the product.

---

# 20. Banner & Content Decisions

## 20.1 Banner classification

Banner is an **application/content feature**, not a core commerce module package.

## 20.2 Banner responsibilities

A banner can contain presentation-oriented information such as:

- image/media;
- title/short text;
- display order;
- active/inactive state;
- scheduling/publishing rules where required;
- destination/action;
- metadata.

## 20.3 Navigation

A banner can be configured to lead to a supported Rehla destination, such as a Visa Service detail surface.

The destination resolution remains a Web/Admin/API concern rather than a justification for creating a giant generic CMS module.

---

# 21. Search Decisions

Search is a supporting platform capability.

It may index resources such as:

- Visa Services;
- relevant content;
- other approved searchable resources.

Search indexing is not the source of truth for business data.

The domain module remains authoritative; search is a projection/discovery layer.

---

# 22. Translation / Localization Decisions

Rehla requires localization support appropriate for its customer audience.

The system must support localized application/content resources without duplicating the underlying business entity per language.

The current product context requires **Arabic and English** support.

The translation layer should remain a reusable platform capability and must not become entangled with a single Admin page or Web component.

---

# 23. File Storage Decisions

## 23.1 Separation

File storage is infrastructure, not business-domain persistence.

```text
Business resource
      ↓
File reference
      ↓
File abstraction
      ↓
Provider
      ↓
Object/file storage
```

## 23.2 Sensitive files

Applicant documents and payment receipts are sensitive operational files.

They require:

- authenticated access;
- authorization checks;
- non-public storage behavior;
- generated storage keys;
- safe file validation;
- lifecycle/history controls where required.

---

# 24. Notification Decisions

Notification is a provider-based infrastructure/domain capability.

The core application should express **what happened** and the notification system should handle **how the message is delivered**.

Conceptually:

```text
Application Event
      ↓
Notification Request
      ↓
Notification Provider
      ↓
Customer-facing channel
```

Notification channel/provider implementation must not become the owner of application state.

---

# 25. Analytics & Reporting Decisions

Analytics exists to provide visibility into operational and product behavior.

It must not become a second transactional database.

Business truth remains in the relevant domain modules.

Operational reporting can include:

- application volumes;
- status distributions;
- payment review workload;
- document review workload;
- service performance;
- customer activity;
- operational turnaround measurements.

Exact dashboards can evolve without redefining the business domain.

---

# 26. Infrastructure Decisions

## 26.1 Logical deployment

The logical production shape is:

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
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐ ┌───────▼──────┐ ┌─────▼─────────┐
│ PostgreSQL   │ │ Redis        │ │ File/Object   │
│ system       │ │ cache/locks  │ │ storage       │
│ of record    │ │ /runtime     │ │               │
└──────────────┘ └──────────────┘ └───────────────┘

                 ┌───────────────┐
                 │     Admin     │
                 └───────┬───────┘
                         │
                         └──────────→ Rehla API
```

## 26.2 Database

PostgreSQL is the system-of-record database in the target deployment architecture.

## 26.3 Redis

Redis is used for infrastructure concerns such as:

- caching;
- locking;
- distributed/runtime support where required.

Redis must not be treated as the source of truth for transactional domain data.

## 26.4 File/object storage

Applicant documents and payment receipts are stored through the File abstraction using file/object storage rather than embedding binary files directly into domain records.

---

# 27. Security Decisions

## 27.1 Security principle

Authorization is a backend responsibility.

## 27.2 Customer isolation

Every customer-owned resource access path must be scoped to the authenticated customer context.

## 27.3 Admin isolation

Admin endpoints/actions require the appropriate staff identity and permissions.

## 27.4 State transition protection

Application, payment, and document transitions are protected server-side.

## 27.5 Sensitive operations

The following are explicitly sensitive:

- payment verification/rejection;
- document review;
- document access;
- application status transitions;
- Admin account changes;
- role/permission changes;
- protected settings.

## 27.6 File security

The system must not trust client-provided storage paths or arbitrary file locations.

## 27.7 Idempotency

Operations that can be retried or submitted repeatedly, especially payment receipt submission and other high-value workflows, should be designed for safe idempotent behavior.

---

# 28. Testing & Quality Decisions

## 28.1 Required test levels

```text
Unit
  ↓
Integration
  ↓
HTTP/API
  ↓
Admin/Web component
  ↓
Critical-path E2E
```

## 28.2 Module testing

A module should be testable independently where practical.

## 28.3 Integration testing

Cross-module behavior must be covered at the integration boundary.

Examples:

```text
Product + Pricing
Application + Documents
Application + Payment
Application + Tracking
Tracking + Notification
```

## 28.4 End-to-end customer journey

The highest-value E2E flow is:

```text
Discover published service
   ↓
Select service
   ↓
Create/continue customer session
   ↓
Create application
   ↓
Submit required data/documents
   ↓
Submit payment evidence
   ↓
Admin verifies payment
   ↓
Admin processes application
   ↓
Customer observes tracking/status
   ↓
Application reaches completion
```

## 28.5 Required quality gates

A feature is not complete merely because code compiles.

A completed package must have appropriate:

- automated verification;
- integration verification where relevant;
- authorization verification;
- API contract verification;
- UI behavior verification;
- critical-path regression coverage where relevant.

---

# 29. Development Rules

## 29.1 Domain-first rule

Use Rehla's business terminology first.

Do not rename concepts solely to match a framework or upstream project.

## 29.2 Smallest-compatible-reuse rule

When borrowing a pattern, use the smallest compatible abstraction that solves the actual Rehla problem.

## 29.3 No accidental domain expansion

Do not introduce a new module because a similar-looking domain exists in Medusa.

Ask whether the behavior actually belongs to Rehla's business model. The established exclusions are binding.

## 29.4 No cross-module internals

Do not directly import another module's private persistence or implementation details.

Use:

- public service contracts;
- workflows;
- events;
- links.

## 29.5 Backend before UI

The implementation order for a new domain capability is:

```text
Schema / Model
      ↓
Module Service
      ↓
Workflow / Event / Link
      ↓
API Contract
      ↓
Admin / Web
      ↓
Automated Verification
      ↓
Manual Verification
```

## 29.6 No business logic in UI

The UI may validate obvious input and guide users, but it cannot be the authoritative source for:

- prices;
- permissions;
- legal state transitions;
- payment verification;
- document acceptance.

## 29.7 No unrelated refactoring

A task should not silently rewrite unrelated modules simply because they are nearby or use an old style.

---

# 30. AI Coding Agent Decisions

Rehla is intended to be developed with disciplined agent-friendly boundaries.

An AI coding agent working in the repository must treat this document as its primary project context.

## 30.1 Before editing

An agent must know:

- what domain it is changing;
- which module owns the domain behavior;
- which public contracts are available;
- which adjacent modules are dependencies;
- which behavior is explicitly excluded.

## 30.2 During implementation

Agents must:

- stay within the requested module/work package;
- use public contracts across module boundaries;
- preserve existing decisions;
- avoid speculative features;
- avoid copying unrelated upstream code;
- update API contracts when transport behavior changes;
- add or update tests with behavior changes;
- preserve authorization boundaries.

## 30.3 After implementation

Agents must verify:

```text
Build
  ↓
Lint / format
  ↓
Unit tests
  ↓
Integration tests
  ↓
API tests
  ↓
Relevant Admin/Web checks
  ↓
Critical-path regression
```

## 30.4 Change discipline

Every architectural change should state:

```text
What changed?
Why was it needed?
Which existing decision does it modify?
What new boundary does it create?
What existing behavior must remain unchanged?
```

---

# 31. Dependency / Build Order Decisions

The established implementation sequence is:

```text
00 Foundation
      ↓
01 Architecture
      ↓
02 Medusa Compatibility Boundary
      ↓
03 Authentication
      ↓
04 Store + Customer + User + RBAC
      ↓
05 Product = Visa Service
      ↓
06 Pricing + Currency + Region
      ↓
07 Service Cart
      ↓
08 Visa Application
      ↓
09 Documents + File
      ↓
10 Payment + Bank Transfer
      ↓
11 Tracking + Notifications
      ↓
12 Banners + Content
      ↓
13 Search + Translation + Settings
      ↓
14 Workflows + Events + Jobs + Links
      ↓
15 Admin Shell
      ↓
16 Admin Resources
      ↓
17 Web Storefront
      ↓
18 Testing + Security + Quality
      ↓
19 Observability + Analytics
      ↓
20 Infrastructure + Staging
      ↓
21 Production Release
```

This order is a dependency-oriented execution sequence. Once contracts are stable, independent work may proceed in parallel without changing ownership boundaries.

---

# 32. Complete Domain Relationship Model

```text
                           ┌─────────────────┐
                           │      Store      │
                           └────────┬────────┘
                                    │
                         ┌──────────▼─────────┐
                         │      Customer      │
                         └──────────┬─────────┘
                                    │
                         ┌──────────▼─────────┐
                         │   Service Cart     │
                         └──────────┬─────────┘
                                    │
                         ┌──────────▼─────────┐
                         │ Product = Visa     │
                         │ Service            │
                         └──────────┬─────────┘
                                    │
                         ┌──────────▼─────────┐
                         │ Visa Application   │
                         └────┬─────┬─────┬───┘
                              │     │     │
                  ┌───────────┘     │     └────────────┐
                  ↓                 ↓                  ↓
          ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
          │  Documents   │  │   Payment    │  │   Tracking   │
          └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
                 │                 │                 │
                 ↓                 ↓                 ↓
             File store       Bank Transfer      Notification

Admin User
    ↓
 Role
    ↓
Permission
    ↓
Protected Admin Resource

Banner / Content
    ↓
Storefront presentation / navigation
```

---

# 33. Responsibility Matrix

| Concern | Customer | Admin | API | Domain Module | Workflow/Event | Infrastructure |
|---|---:|---:|---:|---:|---:|---:|
| Browse services | ✓ | ✓ | ✓ | ✓ |  |  |
| View service details | ✓ | ✓ | ✓ | ✓ |  |  |
| Add to cart | ✓ |  | ✓ | ✓ |  |  |
| Create application | ✓ | ✓/operational | ✓ | ✓ | ✓ |  |
| Submit applicant data | ✓ |  | ✓ | ✓ |  |  |
| Upload documents | ✓ | ✓/review | ✓ | ✓ |  | ✓ |
| Review documents |  | ✓ | ✓ | ✓ |  | ✓ |
| Submit bank receipt | ✓ |  | ✓ | ✓ | ✓ | ✓ |
| Verify payment |  | ✓ | ✓ | ✓ | ✓ |  |
| Change application status |  | ✓ | ✓ | ✓ | ✓ |  |
| Record tracking history |  |  | ✓ | ✓ | ✓ |  |
| Send notification |  |  |  |  | ✓ | ✓ |
| Manage Visa Services |  | ✓ | ✓ | ✓ |  |  |
| Manage banners |  | ✓ | ✓ | feature boundary |  |  |
| Manage Admin users |  | ✓ | ✓ | ✓ |  |  |
| Enforce permissions |  | ✓ | ✓ | ✓ |  |  |
| Search/index |  | ✓ | ✓ | source of truth | ✓ | ✓ |
| Cache/lock |  |  |  |  |  | ✓ |

---

# 34. What Rehla Is Not

This section exists to prevent scope drift.

Rehla is **not**:

```text
A flight reservation system
A hotel booking engine
A travel-agency marketplace
A warehouse management system
A shipping platform
A physical-goods commerce system
A generic fulfillment platform
A Medusa fork
A copy of the Medusa repository
```

The platform can evolve later, but those capabilities require explicit decisions rather than arriving as side effects of architecture reuse.

---

# 35. Explicit Technology / Architecture Decisions

| Decision | Current choice |
|---|---|
| Repository | Rehla-owned monorepo |
| Architecture | Modular monolith |
| Backend | TypeScript / Node modular application |
| Customer Web | Next.js |
| Admin | React/Vite-like operational application |
| Database | PostgreSQL |
| Runtime infrastructure | Redis for cache/locking/runtime support where required |
| File storage | File abstraction + object/file storage |
| Commerce Product | Product = Visa Service |
| Operational request | Application |
| Payment | Generic Payment abstraction + Bank Transfer provider |
| Documents | Dedicated Rehla module + File abstraction |
| Tracking | Dedicated Rehla module |
| Notifications | Dedicated notification capability/provider architecture |
| Banners | Application/content feature |
| Shared UI | Generic design system only |
| Auth | Dedicated authentication boundary |
| Authorization | RBAC + backend enforcement |
| Search | Dedicated search abstraction |
| Translation | Dedicated translation/localization capability |
| Workflows | Cross-module business orchestration |
| Events | Decoupled asynchronous reactions |
| Links | Explicit cross-module entity relationships |
| Caching | Dedicated cache abstraction |
| Locking | Dedicated locking abstraction |

---

# 36. Historical Decisions That Must Not Be Accidentally Revived

Rehla has had multiple architecture explorations during planning. Older alternatives are retained here only to prevent accidental mixing of incompatible designs.

## 36.1 Laravel modular-monolith branch

An earlier planning branch explored Laravel 13 with `nwidart/laravel-modules` and Laravel package-style modules.

**Current status:** superseded by the current TypeScript/Node Rehla architecture described in this document.

Do not combine the Laravel module system with the current TypeScript/Node repository without an explicit architecture decision.

## 36.2 NestJS + Prisma MVP branch

An earlier MVP design described a NestJS API, Prisma persistence, a Next.js dashboard, and a pnpm workspace.

**Current status:** superseded where it conflicts with the current Rehla-owned modular architecture and Admin boundary.

The durable business decisions from that exploration remain valid where they agree with this document, especially the customer-to-application-to-document-to-bank-transfer-to-tracking workflow.

## 36.3 Generic e-commerce / marketplace expansion

Previous explorations considered broader commerce concepts.

**Current status:** not part of the baseline.

Do not reintroduce:

- agencies/vendors;
- marketplace offers;
- flights;
- hotels;
- fulfillment;
- physical inventory.

---

# 37. Non-Decisions and Change Control

Some implementation details are intentionally **not frozen by this document**.

Examples include:

- exact deployment topology beyond the logical architecture;
- exact cloud vendor;
- exact CI provider;
- exact search engine implementation;
- exact notification delivery provider;
- exact object storage vendor;
- exact authentication provider;
- exact framework-internal file layout when module contracts remain intact.

These are implementation choices unless a future decision explicitly promotes them to an architectural requirement.

## 37.1 Rule for changing a decision

When a current decision changes, the change must be made explicitly in this document.

A valid update should identify:

```text
Decision being changed
Previous value
New value
Reason
Affected domains
Migration impact
Compatibility impact
Testing impact
```

No developer should silently reinterpret the product boundary through implementation.

---

# 38. Definition of Done for Rehla Features

A feature is considered complete only when all applicable conditions are true:

### Domain

- The correct module owns the business behavior.
- The domain model reflects the actual Rehla concept.
- No unrelated domain has been introduced.

### API

- The public contract is defined.
- Validation is server-side.
- Authorization is server-side.
- Errors are handled consistently.

### Admin

- The correct resource/action exists.
- Permissions are enforced.
- UI does not bypass business rules.
- Important operational actions are visible/auditable.

### Web

- Customer journey is coherent.
- Ownership boundaries are enforced.
- API responses are handled correctly.

### Data

- Persistence belongs to the owning module.
- Historical snapshots are used where required for integrity.
- Sensitive data/file access is protected.

### Async behavior

- Events/workflows/jobs are used only when justified.
- Retries are safe.
- Idempotency exists where duplicate submission is possible.

### Verification

- Relevant tests pass.
- Cross-module integration is verified.
- Security-sensitive paths are verified.
- Critical-path regression is considered.

---

# 39. CRUD vs Domain Architecture Decisions

## 39.1 Rehla is not a CRUD-only application

Rehla uses CRUD operations as a **data-management capability inside domain modules**, but CRUD is not the application's overall architecture.

The correct architectural model is:

```text
Customer / Admin
        ↓
       API
        ↓
    Workflow
        ↓
      Module
        ↓
     Service
        ↓
   Data Model
        ↓
   PostgreSQL
```

CRUD exists primarily at the **module/service data-management layer**.

The platform must not be reduced to:

```text
Controller
   ↓
CRUD
   ↓
Database
```

## 39.2 What CRUD means in Rehla

When a domain entity requires ordinary data management, its owning module may provide operations such as:

```text
Create
Retrieve / Get
List
Update
Delete
```

These operations are module-owned.

Conceptually:

```text
Product Module
├── Product Model
└── Product Service
    ├── create
    ├── retrieve
    ├── list
    ├── update
    └── delete
```

The same pattern can be used where appropriate for other resources.

However:

> **Not every entity needs every CRUD operation, and not every operation should be exposed as a generic CRUD endpoint.**

The allowed surface must follow the domain rules.

## 39.3 CRUD is subordinate to domain ownership

The module owns the data and business behavior.

Therefore:

```text
CRUD
  = data-management capability

Module
  = domain ownership boundary

Service
  = domain/data operations

Workflow
  = multi-step business orchestration

Event
  = asynchronous reaction / side effect

API
  = transport and public application boundary
```

The presence of CRUD does not remove the need for domain services, validation, authorization, state machines, workflows, events, links, or background jobs.

## 39.4 Rehla resource classification

Rehla resources are classified into three practical groups.

### Group A — CRUD-oriented resources

These resources are primarily administrative/data-management resources:

```text
Customers
Visa Services / Products
Categories
Banners / Content
Admin Users
Settings
```

Their common operations may include:

```text
List
View
Create
Edit
Publish / Unpublish where applicable
Delete / Archive where the domain allows it
Search
Filter
```

Even these resources remain subject to authorization, validation, auditability, and domain constraints.

### Group B — CRUD + domain-action resources

These resources require normal data access **plus explicit business actions**:

```text
Applications
Documents
Payments
Tracking
```

For example, Application is not simply:

```text
Create Application
Read Application
Update Application
Delete Application
```

It also has domain operations such as:

```text
Create Application
Submit Application
Request Documents
Review Application
Change Status
Approve
Reject
Complete
```

Likewise, Payment includes operations such as:

```text
Create / Initiate Payment Record
Submit Receipt
Review Payment
Verify Payment
Reject Payment
```

Documents include operations such as:

```text
Submit Document
Review Document
Accept Document
Reject Document
Request Replacement
```

Tracking includes operations such as:

```text
Record Status Change
Add Tracking Update
Read Timeline
Expose Customer-visible Progress
```

These actions are not interchangeable with generic `UPDATE` operations.

## 39.5 Workflow-oriented behavior

Some Rehla behavior crosses module boundaries and therefore belongs in workflows rather than in a generic CRUD endpoint.

Examples:

```text
Create Visa Application
        ↓
Application
        ↓
Documents
        ↓
Payment
        ↓
Tracking
        ↓
Notification
```

and:

```text
Verify Payment
        ↓
Validate permission/state
        ↓
Verify receipt
        ↓
Update Payment
        ↓
Apply required Application transition
        ↓
Record Tracking history
        ↓
Emit business event
        ↓
Notify Customer
```

A workflow may coordinate multiple module operations while preserving module ownership.

## 39.6 Admin is not a CRUD generator

The Admin UI may present many resources using familiar CRUD-oriented interaction patterns:

```text
List
  ↓
Detail
  ↓
Create / Edit
  ↓
Actions
```

But the Admin is an **operational application**.

It must expose domain actions where the domain requires them, for example:

```text
Applications
├── Review
├── Request documents
├── Change status
└── Complete / Reject where legally valid

Payments
├── Inspect receipt
├── Verify
└── Reject

Documents
├── Review
├── Accept
├── Reject
└── Request replacement

Tracking
└── Publish operational progress update
```

The Admin must not implement these business rules itself.

The correct boundary is:

```text
Admin UI
   ↓
API action
   ↓
Domain Service / Workflow
   ↓
Module
   ↓
Persistence
```

## 39.7 No generic CRUD shortcut around business rules

The following pattern is prohibited for business-critical state:

```text
Admin UI
   ↓
PATCH /applications/:id
   ↓
UPDATE applications
SET status = ...
```

Instead:

```text
Admin UI
   ↓
Change Application Status action
   ↓
API
   ↓
Application workflow / domain service
   ↓
Validate legal transition
   ↓
Persist new state
   ↓
Record immutable history
   ↓
Emit event / trigger notification when required
```

This rule applies especially to:

- application status;
- payment verification/rejection;
- document review;
- sensitive permission changes.

## 39.8 CRUD does not define the HTTP API

The HTTP API should expose resource operations and business actions according to the domain.

Conceptually:

```text
Resource endpoints
    +
Domain action endpoints
    +
Workflow entry points
```

The API must not be designed as a blanket reflection of database tables.

The database schema is an internal implementation boundary owned by modules; the public API is a product contract.

## 39.9 CRUD, transactions, and idempotency

CRUD-style writes do not remove the need for:

- transactional boundaries;
- concurrency protection;
- authorization;
- validation;
- idempotency where duplicate submissions are possible;
- audit/history;
- workflow compensation where applicable.

High-value operations such as payment receipt submission and status transitions require safe retry semantics.

## 39.10 Developer rule

When adding a new Rehla feature, first decide which of these applies:

```text
A. CRUD data management
B. CRUD + domain action
C. Cross-module workflow
D. Event / subscriber
E. Background job
F. Infrastructure capability
```

Do not default every feature to CRUD.

The implementation must place the behavior at the lowest correct ownership boundary while preserving the public API and security model.

## 39.11 Final architecture interpretation

The correct mental model for Rehla is:

```text
                    REHLA
                      │
               Modular Monolith
                      │
         ┌────────────┴────────────┐
         │                         │
     CRUD / Data              Domain Behavior
     Management               & Orchestration
         │                         │
         └────────────┬────────────┘
                      │
                  Modules
                      │
                ┌─────┴─────┐
                │           │
             Services    Workflows
                │           │
                └─────┬─────┘
                      │
                 Events / Links
                      │
                     API
                ┌─────┴─────┐
               Admin       Web
```

Therefore:

> **Rehla is a modular-monolith domain platform that includes CRUD, but is not a CRUD application.**


# 40. Final Decision Summary

The complete Rehla baseline can be reduced to the following rules:

```text
1. Rehla is a visa-service commerce platform for Sudanese travelers.

2. Product means the sellable Visa Service.

3. Application means the customer's actual visa-service request.

4. Rehla is a Rehla-owned modular monolith.

5. Rehla lives in its own monorepo.

6. Medusa is an architectural/reference source, not Rehla's owner.

7. Reuse must be selective and behavior-compatible.

8. The customer flow is:
   discover → service → cart → account → application → documents
   → bank transfer → receipt → review → processing → tracking → completion.

9. Payment is bank transfer with manual receipt verification.

10. Documents are domain records; files are storage resources.

11. Tracking is a first-class Rehla domain.

12. Notifications react to important business events.

13. Admin is an operational application, not the owner of business logic.

14. Admin access is protected by backend-enforced RBAC.

15. Web and Admin communicate through the Rehla API.

16. @rehla/ui remains generic; business resources remain in Admin.

17. Banners are application/content features, not core module packages.

18. Flights, hotels, agencies/marketplace, fulfillment, shipping, and
    physical inventory are outside the baseline.

19. Cross-module relationships use explicit contracts, links, workflows,
    and events instead of private implementation coupling.

20. PostgreSQL is the system of record; Redis supports infrastructure;
    file/object storage handles documents and receipts.

21. Backend authorization is authoritative; UI guards are not security.

22. Business state transitions are enforced server-side and recorded in
    immutable history.

23. Every major capability must be independently testable and integrated
    through explicit contracts.

24. Any architectural or product change must be made explicitly in this
    decision record rather than silently inferred during implementation.
```

---

<div align="center">

**Rehla · رحله**  
**One product boundary. One domain model. Explicit decisions.**

</div>
