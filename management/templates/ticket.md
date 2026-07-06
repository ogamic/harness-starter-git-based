# Ticket templates

Copy one of these into `backlog/NNNN-slug.md` or `bugs/NNNN-slug.md`. Use the **light** template for small items; the **PRD-style** template for meaty, multi-phase ones.

---

## Light template (small items)

```markdown
# Backlog/Bug NNNN: <one-line title>

**Status:** Open | In Progress | Awaiting Owner | Done
**Priority:** Low | Medium | High | Critical
**Surfaces:** api / app / android
**Opened:** YYYY-MM-DD
**Reported by:** Owner | user | monitoring

## Context

<1–3 sentences — why this matters, how we noticed it>
**Auto-persisted:** Yes — rubric (1 surface / correction / no call / internal) · or · No — Owner approved

## Plan

- <agent>: <what they do>
- Out of scope: <anything deliberately NOT touched>

## Outcome

<!-- filled post-execution -->
- Files changed: `<path:line>`
- Verified via: <build / tests with counts / manual smoke>
- Evidence: <what proved it works>
- Harness delta: <what this taught the system, or "None">
```

---

## PRD-style template (meaty items)

```markdown
# Backlog NNNN: <title>

**Status:** …  ·  **Priority:** …  ·  **Surfaces:** …  ·  **Opened:** YYYY-MM-DD
**Epic:** <name, if part of one>

## Context / problem
What we're solving and why now. Non-obvious constraints.

## Goal & non-goals
- Goal: …
- Non-goal (explicitly out of scope): …

## Plan (by phase)
1. **Phase 1 — <agent>:** …
2. **Phase 2 — <agent>:** …

## Contract
Any shape a consumer relies on — field/param names, permissions, URLs — pinned here BEFORE parallel work starts.

## Outcome
<!-- filled per phase as work lands -->
- Phase 1: <files, evidence>
- Harness delta: …
```
