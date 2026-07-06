# Architecture — overview

*Example stub. Describe how this service is actually built; the owning agent reads this first.*

- **Shape:** a REST API — routes → controllers → services → data access.
- **Data:** Postgres, one schema per bounded context; migrations are versioned and forward-only.
- **Auth:** token-based; permissions checked at the route boundary.
- **Config:** environment variables, documented in `.env.example`; never commit secrets.

*Last updated: 2026-01-01 — keep this stamp current in the same edit that changes content.*
