# Rehla Development Plans

Execution order is numerical from Foundation to Production Release.

All plans follow the current Rehla decisions: one Store; Product is the catalog primitive and a Visa Service is a Product; Customer and Admin User are separate actors; the service-commerce path is `Cart → Application`; and Banner is an application/content capability, not a module under `packages/modules/`. Rehla adopts selected Medusa patterns and compatible packages while remaining an independent repository. See [the constitution](../constitution.md), [ADR 001](../decisions/001-medusa-without-copying.md), and [the dependency graph](../dependency-graph.md).

- [00 — Foundation and Monorepo](./00-foundation.md)
- [01 — Architecture and Module Boundaries](./01-architecture.md)
- [02 — Medusa Compatibility and Selective Reuse](./02-medusa-compatibility.md)
- [03 — Customer and Admin Authentication](./03-auth.md)
- [04 — Store, Customer, Admin User, and RBAC](./04-store-customer-user-rbac.md)
- [05 — Product Catalog as Visa Services](./05-product-visa.md)
- [06 — Pricing, Currency, and Destination Context](./06-pricing-currency-region.md)
- [07 — Service Cart](./07-cart.md)
- [08 — Visa Service Applications](./08-application.md)
- [09 — Documents and File Storage](./09-documents-file.md)
- [10 — Payment and Bank Transfer](./10-payment-bank-transfer.md)
- [11 — Tracking and Notifications](./11-tracking-notifications.md)
- [12 — Banners and Storefront Content](./12-banners-content.md)
- [13 — Search, Translation, and Settings](./13-search-translation-settings.md)
- [14 — Workflows, Events, Jobs, Locking, Caching, and Links](./14-workflows-events-jobs.md)
- [15 — Admin Shell](./15-admin-shell.md)
- [16 — Admin Resources](./16-admin-resources.md)
- [17 — Web Storefront](./17-web-storefront.md)
- [18 — Testing, Security, and Quality](./18-testing-quality-security.md)
- [19 — Observability and Analytics](./19-observability-analytics.md)
- [20 — Infrastructure, Deployment, and Staging](./20-infrastructure-deployment.md)
- [21 — Production Release](./21-production-release.md)
