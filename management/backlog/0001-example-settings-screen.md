# Backlog 0001: Add a settings screen to the web app

> **This is an example ticket** shipped with the starter to show the format. Delete it when you file your first real one.

**Status:** Open
**Priority:** Medium
**Surfaces:** app
**Opened:** 2026-01-01
**Reported by:** Owner

## Context

The web app has no place for users to change account preferences (theme, notifications). This is a new user-facing screen.

**Auto-persisted:** No — Owner approved. (Fails the rubric: it's an *Addition*, not a correction, so it needs a go before persisting.)

## Plan

- `app`: add a `/settings` route with a preferences form (theme toggle, notification opt-in). Persist via the existing preferences endpoint; no new backend.
- Out of scope: adding new preference *fields* to the API (that's a contract change — separate ticket if needed).

## Outcome

<!-- Filled in post-execution by the PM from the sub-agent's evidence. -->

- Files changed: `<path:line>`
- Verified via: `<build / tests with counts / manual smoke>`
- Evidence: `<what proved it works>`
- Harness delta: `<what this taught the system, or "None">`
