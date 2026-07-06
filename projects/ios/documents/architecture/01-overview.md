# Architecture — overview

*Example stub. Describe how this client is actually built; the owning agent reads this first.*

- **Shape:** SwiftUI views → observable view models → a service layer that calls the `api` service.
- **State:** unidirectional; views observe immutable state; avoid scattered singletons where a scoped model fits.
- **Concurrency:** `async/await`; keep UI work on the main actor.
- **Data:** `Codable` models mirroring the API contract; decode defensively so an unknown value never crashes the app.

*Last updated: 2026-01-01 — keep this stamp current in the same edit that changes content.*
