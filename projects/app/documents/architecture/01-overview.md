# Architecture — overview

*Example stub. Describe how this client is actually built; the owning agent reads this first.*

- **Shape:** component tree → routed views → a data layer that calls the `api` service.
- **State:** local component state by default; a shared store only where genuinely cross-cutting.
- **Styling:** Tailwind utilities; shared design tokens over ad-hoc values.
- **Data:** typed client for the API; never hardcode response shapes the API owns.

*Last updated: 2026-01-01 — keep this stamp current in the same edit that changes content.*
