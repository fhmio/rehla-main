# ADR 001 — Medusa-Inspired Rehla Without Copying Medusa

## Decision

Rehla follows selected Medusa modular architecture and Admin UX patterns while remaining an independent repository.

Compatible Medusa packages are consumed as dependencies after review. A package is localized under `packages/modules` only when Rehla needs source-level customization. Rehla-specific capabilities are implemented locally using the approved module contract.

The current Rehla product has one Store. `Product` is the catalog primitive; a Visa Service is a Product with Rehla-specific fields, not a separate Visa commerce module. `Customer` is the storefront/business actor and `User` is the separate Staff/Admin actor.

The service-commerce path is `Cart → Application`. Generic Order, shipping, and Fulfillment capabilities are outside the current core.

`Banner` is an application/content capability, not a module under `packages/modules/`. The Admin application may adopt selected resource, layout, widget, and design-system patterns, while Rehla owns its Admin resources, business logic, and product behavior.

Use Links for cross-domain associations, Workflows for multi-domain commands, and Events/Jobs for asynchronous side effects.

## Explicitly Excluded From the Core

Order, Fulfillment, Inventory, Stock Location, Sales Channel, Tax, Promotion, flight booking, hotel booking, and travel-agency marketplace.