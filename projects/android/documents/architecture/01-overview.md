# Architecture — overview

*Example stub. Describe how this client is actually built; the owning agent reads this first.*

- **Shape:** MVVM — Compose UI → ViewModel → repository → the `api` service.
- **State:** unidirectional; UI observes immutable state exposed by the ViewModel.
- **DI:** constructor injection; a single graph wired at the app boundary.
- **Data:** typed models mirroring the API contract; decode defensively.

*Last updated: 2026-01-01 — keep this stamp current in the same edit that changes content.*
