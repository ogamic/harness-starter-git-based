---
name: ticket-new
description: Use when opening a new backlog or bug ticket. Enforces search-before-open, derives the ID from the folder (not STATUS), runs the 4-signal autonomy rubric to decide auto-vs-ask, writes the file from the template, and updates STATUS.md.
---

# ticket-new — open a ticket with the scoping discipline enforced

The playbook's scoping rules are exactly the ones that get skipped under momentum. This skill makes them mandatory steps, in order.

## Procedure

1. **Search before you open.** Scan `backlog/STATUS.md` (or `bugs/STATUS.md` for a bug) — the **Open**, **Awaiting Owner**, and **Epics** sections — for the same surface/feature/bug. Then grep both folders: `grep -ril <keyword> backlog/ bugs/`.
   - If a related ticket exists → **extend it or add a phase. Do not open a sibling.** Report the match and stop for the Owner's call unless it's an obvious phase.
   - If it belongs under an Epic → attach it there, don't spawn a loose sibling.
2. **Derive the next ID from the folder, never from STATUS.** `ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1` → increment. The `grep` skips `STATUS.md`, which otherwise sorts last and hands you it as the "previous" ID; empty output = no tickets yet, so start at `0001`. STATUS.md is hand-maintained and lags; trusting it for IDs risks a collision.
3. **Pick the template** from `templates/ticket.md`: the **light** template for small items, the **PRD-style** template for meaty / multi-phase ones.
4. **Run the 4-signal autonomy rubric** (pm-playbook → "Autonomy rubric"):
   - **Blast radius** — 1 surface = auto · cross-surface = ask
   - **Change type** — correction (fix/typo/revert) = auto · addition (new feature/endpoint/screen) = ask
   - **Product decision** — none/clear = auto · any "A or B?" = ask
   - **Contract** — internal = auto · anything a consumer relies on (API shapes, permissions, CLI flags, schema, URLs) = ask
   - **All four Auto** → persist and delegate now; in the ticket's **Context**, name the line that authorized it: `Auto-persisted: rubric (1 surface / correction / no call / internal)`.
   - **Any Ask** → draft the ticket, present the plan, get the Owner's approval *before* persisting.
5. **Write** `backlog/NNNN-slug.md` (or `bugs/NNNN-slug.md`). One ticket = one coherent work item — if the title needs "and", split it; multi-step within one unit = phases in the body.
6. **Add the row to `STATUS.md`** in the correct lane (a fresh actionable ticket → **Open**).

## Report back

The path written, the rubric verdict (and which line authorized an auto-persist), and — if you found a related ticket in step 1 — what you extended instead of opening.
