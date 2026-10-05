# Rehla Plan Dependency Graph

The graph orders delivery plans. Domain ownership follows `docs/development/decisions/001-medusa-without-copying.md`: one Store, Product/Visa Service, distinct Customer and Admin User actors, `Cart → Application`, and Banner as an application/content capability.

```text
00 Foundation
  ↓
01 Architecture
  ↓
02 Medusa Compatibility
  ↓
03 Auth
  ↓
04 Store + Customer + Admin User + RBAC
  ↓
05 Product / Visa Service
  ↓
06 Pricing + Currency + Region
  ↓
07 Cart
  ↓
08 Application
  ↓
09 Documents + File
  ↓
10 Payment
  ↓
11 Tracking + Notifications
  ↓
12 Banners
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
18 Testing + Security
  ↓
19 Observability + Analytics
  ↓
20 Infrastructure + Staging
  ↓
21 Production Release
```

## Parallelizable Work

Plans 12 and 13 can start after their backend dependencies exist; 19 can start once the event/workflow foundation exists. The overall release order remains 00 → 21.