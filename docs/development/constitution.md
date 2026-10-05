# Rehla Constitution

- Rehla is independent; never add the Medusa repository as a second source tree.
- Store is the commerce context.
- Customer is the storefront/business actor.
- User is the Admin/Staff actor.
- Product is the catalog primitive; Visa Service is Product.
- Cart is service-commerce and has no shipping/fulfillment journey.
- Application is the Rehla business request created from Cart.
- Documents, Payment, Tracking, Notification are independent capabilities.
- Banner is application/content, not `packages/modules`.
- Admin resources live in the Admin application and use generic shared UI.
- No flights, hotels, or travel-agency marketplace in the current scope.
- Cross-module relationships use Links; multi-step transactions use Workflows; side effects use Events/Jobs.
- Every phase must have automated and manual verification.
