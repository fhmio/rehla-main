# Application Boundary Rules

Applies to `apps/*`.

## Rules

- Rehla has one `Store` in the current product scope. `Product` is the catalog primitive and a Visa Service is a Product.
- `Customer` is the storefront actor; `User` is the separate Staff/Admin actor. Keep their authentication and UI contexts distinct.
- The storefront service journey is `Cart → Application`; do not introduce an Order/shipping/Fulfillment journey.
- Banner is an application/content capability surfaced through the approved API boundary, not a domain module under `packages/modules/`.
- Reuse selected Medusa patterns while keeping Rehla applications and business capabilities owned by this repository.

- Applications are consumers of domain capabilities, not owners of domain data.
- Do not access module database tables directly.
- `web` and `admin` communicate with backend capabilities through the approved SDK/API boundary.
- `api` is the composition/runtime boundary for routes, workflows, subscribers, jobs, links, and configuration.
- Do not move business rules from modules/workflows into UI components or HTTP handlers.
- Do not add domain logic to an application merely because it is convenient for the current screen.
- Keep application-specific presentation/state concerns inside the application.
