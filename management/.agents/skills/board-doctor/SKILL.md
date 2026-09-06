---
name: board-doctor
description: Use to audit and reconcile the file board. Detects ID collisions, orphaned tickets, STATUS-vs-body lane drift, and stale Awaiting-Owner items, then rebuilds STATUS.md from the folder (the source of truth). Run it whenever STATUS feels out of sync — the file board's index lags by design.
---

# board-doctor — reconcile the file board with its index

The file board's one structural weakness: `STATUS.md` is maintained by hand, so it **lags** the folder — and a lagging index is how two tickets end up sharing an ID or a done ticket looks open. This skill audits the drift and rebuilds the index.

**Source-of-truth order:** the **folder** decides what tickets *exist*; each ticket's body `**Status:**` decides its lane. `STATUS.md` is a *derived view* for the columns the body carries (ID, title, priority) — but some columns live **only** in STATUS: the Awaiting-Owner lane's `Waiting on` / `Detail`, and the Open lane's `Agents`. Those aren't in any ticket body, so **carry them forward from the current STATUS** — rebuild the derivable columns, preserve the non-derivable ones, and **never edit a ticket body to match STATUS.**

## Checks

Run across `backlog/` and `bugs/`:

1. **ID collisions** — two files sharing an `NNNN`. Report both; the Owner picks who renumbers (renumbering is a rename + a STATUS fix).
2. **Orphans, both directions** — ticket files not listed in STATUS; STATUS rows pointing to a file that doesn't exist.
3. **Lane drift** — a ticket whose body `Status:` (e.g. `Done`) disagrees with its lane in STATUS (e.g. still under Open). The body wins.
4. **Stale Awaiting Owner** — items parked in the Awaiting-Owner lane. **Flag** them for a nudge; do not move them (only the Owner clears that lane).
5. **Next-ID safety** — confirm the max ID in the folder (`ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1`) ≥ the max ID shown in STATUS, so the next `ticket-new` won't collide.

## Procedure

1. Enumerate the ticket files — `ls backlog/ | grep -E '^[0-9]{4}'` and the same for `bugs/` (the `grep` skips `STATUS.md`) — and read each ticket's inline header fields: `**Status:**`, `**Priority:**`, and the title. Tickets use bold headers, not YAML frontmatter — don't look for a `---` fence.
2. Run the five checks; build a findings list.
3. **Propose the reconciled `STATUS.md`.** Rebuild the derivable columns (ID, title, priority, lane) from the folder + body; **preserve the non-derivable columns** (`Waiting on`, `Detail`, `Agents`) verbatim from the current STATUS for every row that still exists — dropping them loses the Owner's tracking. Keep the two lanes' distinct shapes (Open: `ID | Title | Priority | Status | Agents` · Awaiting Owner: `ID | Title | Waiting on | Detail`). Show the diff against the current file. **Confirm before writing** if the rebuild moves or drops anything the Owner might not expect; a pure no-op refresh can just be applied.
4. Write the reconciled `STATUS.md`. Leave ticket bodies untouched.

## Report back

The findings (collisions / orphans / drift / stale / next-ID), what the rebuilt STATUS changed, and anything that needs an Owner decision (renumbering a collision, nudging a stale item).
