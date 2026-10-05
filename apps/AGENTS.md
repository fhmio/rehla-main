# Application Boundary Rules

Applies to `apps/*`.

## Rules

- Applications are consumers of domain capabilities, not owners of domain data.
- Do not access module database tables directly.
- `web` and `admin` communicate with backend capabilities through the approved SDK/API boundary.
- `api` is the composition/runtime boundary for routes, workflows, subscribers, jobs, links, and configuration.
- Do not move business rules from modules/workflows into UI components or HTTP handlers.
- Do not add domain logic to an application merely because it is convenient for the current screen.
- Keep application-specific presentation/state concerns inside the application.
