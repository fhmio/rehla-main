# Rehla Build Roadmap

| # | Plan | Primary dependency |
|---|---|---|
| 00 | [Rehla Foundation and Monorepo](./plans/00-foundation.md) | — |
| 01 | [Rehla Architecture and Module Boundaries](./plans/01-architecture.md) | 00 |
| 02 | [Medusa Compatibility and Selective Reuse](./plans/02-medusa-compatibility.md) | 01 |
| 03 | [Customer and Admin Authentication](./plans/03-auth.md) | 02 |
| 04 | [Store, Customer, Admin User, and RBAC](./plans/04-store-customer-user-rbac.md) | 03 |
| 05 | [Product Catalog as Visa Services](./plans/05-product-visa.md) | 04 |
| 06 | [Pricing, Currency, and Destination Context](./plans/06-pricing-currency-region.md) | 05 |
| 07 | [Service Cart](./plans/07-cart.md) | 06 |
| 08 | [Visa Service Applications](./plans/08-application.md) | 07 |
| 09 | [Documents and File Storage](./plans/09-documents-file.md) | 08 |
| 10 | [Payment and Bank Transfer](./plans/10-payment-bank-transfer.md) | 09 |
| 11 | [Tracking and Notifications](./plans/11-tracking-notifications.md) | 10 |
| 12 | [Banners and Storefront Content](./plans/12-banners-content.md) | 11 |
| 13 | [Search, Translation, and Settings](./plans/13-search-translation-settings.md) | 12 |
| 14 | [Workflows, Events, Jobs, Locking, Caching, and Links](./plans/14-workflows-events-jobs.md) | 13 |
| 15 | [Rehla Admin Shell Using Selected Medusa Patterns](./plans/15-admin-shell.md) | 14 |
| 16 | [Rehla Admin Resources](./plans/16-admin-resources.md) | 15 |
| 17 | [Rehla Web Storefront](./plans/17-web-storefront.md) | 16 |
| 18 | [Testing, Security, and Quality Gates](./plans/18-testing-quality-security.md) | 17 |
| 19 | [Observability and Analytics](./plans/19-observability-analytics.md) | 18 |
| 20 | [Infrastructure, Containers, CI/CD, and Staging](./plans/20-infrastructure-deployment.md) | 19 |
| 21 | [Production Release and Launch](./plans/21-production-release.md) | 20 |

## Binding Product Decisions

- Rehla has one Store in the current scope.
- Product is the catalog primitive; Visa Services are Products, not a separate commerce module.
- Customer and Admin User are distinct actors.
- Service commerce flows from Cart to Application without generic Order, shipping, or Fulfillment.
- Banner is an application/content capability, not a module under `packages/modules/`.

## Final Runtime Shape

```text
Store
├── Customer
│   └── Cart
│       └── Product = Visa Service
│           └── Application
│               ├── Documents → File
│               ├── Payment
│               └── Tracking → Notification
└── Admin User → RBAC

Banner = application/content feature
```

## Deployment Path
`local foundation → verified modules → Admin/Web → security/E2E → staging → production release`
