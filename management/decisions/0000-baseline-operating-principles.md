# ADR 0000: Baseline operating principles

**Status:** Accepted
**Date:** 2026-01-01
**Owner:** <the Owner>

## Context

This is the retroactive baseline — the principles the workspace already runs on, captured so later decisions have something to build on or supersede.

## Decision

1. **The PM never writes implementation code.** It coordinates and delegates; sub-agents build. A PM that codes bypasses each surface's conventions.
2. **The board is files.** Tickets, bugs, and decisions are numbered markdown in `management/`, indexed by `STATUS.md`. `git` is the backend.
3. **Decisions are recorded, not remembered.** Cross-cutting choices become ADRs here, with rationale; supersede rather than rewrite.
4. **A sub-project's `documents/` is its law.** The owning agent reads it first and stops on conflict rather than improvising.
5. **Naming:** `kebab-case` for files and slugs, `ALL_CAPS` for index/status files (`STATUS.md`, `CANDIDATES.md`).

## Alternatives considered

- **A database board from day one:** more power (RBAC, a web door, no index drift), but real infra and an account wall for a single-player workspace. Deferred until a *team* needs it — see pm-playbook → "Graduate to a team board".
- **A PM that also codes:** faster in the moment, but erases the surface-conventions boundary that keeps multi-agent work consistent.

## Consequences

- Zero setup and full `git` history for the board — but `STATUS.md` is hand-maintained and can lag, so IDs are derived from the folder, not the index.
- The operating model is portable: graduating to a shared database changes *where the board lives*, not how the workspace runs.
