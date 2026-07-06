---
name: ingest
description: Use when bringing an existing pile of docs, tickets, or notes (a folder of markdown, an exported wiki, a legacy docs/ tree) INTO this workspace's file board. Classifies each item into backlog / bugs / decisions / project-docs, proposes a mapping for the Owner to approve, then writes. Never bulk-imports blind.
---

# ingest — onboard an existing documents system into the board

Bring a pre-existing pile of docs into the workspace shape (`backlog/`, `bugs/`, `decisions/`, `projects/<name>/documents/`). The hard part is classification, and it is **lossy and judgment-heavy** — so the skill's real product is a *proposed mapping the Owner approves*, not a bulk write.

**Iron rule: classify → confirm → write. Never write before the Owner approves the mapping.**

## Inputs

- A **source path** — a folder of markdown, an exported wiki dump, a legacy `docs/` tree, or an explicit list of files. Ask for it if not given.
- (Optional) a hint about what the source *is* (e.g. "old Notion export", "a repo's loose design notes").

## Procedure

1. **Inventory.** List every candidate file under the source. Read enough of each to classify it — title, first paragraphs, structure. Do not skim past files silently; every file is either mapped or explicitly dropped.
2. **Classify** each item into exactly one bucket:
   - `backlog/` — open work / a thing someone still needs to do.
   - `bugs/` — a defect / something broken.
   - `decisions/` — a *"why we chose X over Y"* — durable rationale. An ADR.
   - `projects/<name>/documents/` — durable spec / architecture / convention that belongs *next to a surface's code*.
   - **Drop** — noise, duplicates, obsolete, or empty. Dropping is a valid outcome; a pile always has some.
3. **Propose the mapping and STOP.** Emit a table — `source file → target path → bucket → one-line rationale` — plus a short list of everything dropped (with why) and everything **ambiguous** (the Owner should eyeball these). Do not write a single file yet. Wait for the Owner's go.
4. **On approval, write:**
   - Allocate IDs from the folder, not from STATUS: `ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1` → next `NNNN` (the `grep` skips `STATUS.md`). Same for `bugs/`.
   - Fill each new ticket/ADR from the templates (`templates/ticket.md`, `decisions/README.md`). Preserve the source's original date and a provenance line (`Ingested from: <original path>`) so nothing is silently rewritten history.
   - Don't invent a `Status` or `Priority` you can't infer — mark `UNKNOWN — Owner to set` rather than guessing.
   - Rebuild `backlog/STATUS.md` and `bugs/STATUS.md` from the folder once all files are written (see `board-doctor` — reuse its reconcile step).
5. **Report back** (data, not prose): counts per bucket, the list of dropped items, the list of items you flagged ambiguous, and any file you couldn't classify. Name anything the Owner must recheck by hand.

## Guardrails

- **Ambiguity is surfaced, never resolved silently.** "Is this a decision or just a note?" → flag it in step 3, let the Owner call it.
- **No scope creep.** Ingest re-homes what exists; it does not improve, merge, or rewrite content. If a doc is wrong, that's a follow-up ticket, not an edit during ingest.
- **Idempotent-ish.** If run twice, detect already-ingested files (by provenance line) and skip them rather than duplicating.
