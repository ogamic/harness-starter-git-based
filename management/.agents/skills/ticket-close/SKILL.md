---
name: ticket-close
description: Use when finishing a ticket. Enforces the evidence bar (tests/build with counts, observed behavior), fills the Outcome section, requires an explicit harness-delta, and moves the lane in STATUS.md. Refuses to close on vibes.
---

# ticket-close — close a ticket with proof, not vibes

"Done" requires evidence logged in the ticket. This skill will **refuse to close** a ticket whose evidence bar isn't met, and it forces the one step everyone forgets: the harness delta.

## Procedure

1. **Check the evidence bar** (pm-playbook → "Evidence bar"). To close, the sub-agent's report must include:
   - Tests green **with counts**, or build green **with the tool named**.
   - Behavior **observed where it runs** — dev-server smoke, compiled-binary boot, DOM render, a real request — matched to the surface.
   - If any is missing → **do not close.** State exactly what's missing and leave the ticket In Progress. Cheap-to-verify claims: the PM re-smokes them independently rather than relaying the agent's word.
2. **Fill the Outcome section** of the ticket file:
   - `Files changed:` `<path:line>`
   - `Verified via:` build / tests-with-counts / manual smoke
   - `Evidence:` what actually proved it works
3. **Write the harness delta — mandatory.** Answer *"what did this ticket teach the system?"* — a playbook rule that was wrong, a spec gap, a rubric edge case. Then **act on it now**: fold it into the playbook, a decision, or a new ticket. `"None"` is a valid answer, but it must be written explicitly — absence is not an answer.
4. **Move the lane in `STATUS.md`:**
   - Fully done and needs nothing from the Owner → **Closed**.
   - Built/decided but needs the Owner's commit / deploy / review / decision → **Awaiting Owner** (this is *not* an open-work lane).
5. **Freeze the body.** Once a ticket is Done its body is an immutable record. New related work is a **new ticket** (run `ticket-new`), not an edit to the closed history.

## Report back

The lane it moved to, the evidence that cleared the bar (with counts), and the harness delta — including what you folded it into.
