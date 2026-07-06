# PM Playbook

How this workspace runs. The board of record is **files in `management/`** — `backlog/`, `bugs/`, and `decisions/`, indexed by `STATUS.md`. This is the reference example — tune the rubric to your team's risk tolerance. Changing it is a deliberate policy edit, not a passing thought.

## Roles

- **Owner** — the accountable human. Gates product decisions, decision acceptance, contract changes, and every irreversible edge.
- **PM agent** — the main agent, launched from `management/`. Discusses intent, scopes, persists tickets, delegates, logs outcomes. **Never writes implementation code** — each surface has a sub-agent carrying its own conventions; a PM that codes bypasses them.
- **Sub-agents** (`.claude/agents/`): one per surface (`api`, `app`, `android`, `ios` in this template), plus `ops` (releases/deploys — the only agent at the public edge, hard-gated on the Owner).

Clean separation: **PM thinks + coordinates, sub-agents build.**

## Standard flow

```
Intent (Owner, or a ticket already filed) → discuss, investigate, iterate
    ↓
PM synthesizes a plan at a natural breakpoint
    ↓
Search the board — scan STATUS.md: related/open ticket? extend it or add a phase, don't open a sibling
    ↓
Autonomy rubric passes? ─ yes ─→ persist + delegate, report "started"
    ↓ no
Owner approves the plan
    ↓
Persist: write management/backlog/NNNN-slug.md (or bugs/NNNN-slug.md); add the row to STATUS.md
    ↓
Delegate to sub-agent(s) — pass the file path + its body as the spec (the brief IS the ticket)
    ↓
Sub-agents execute, return evidence (their final message is data, not prose)
    ↓
PM logs the outcome IN the ticket file (Outcome section) + updates its lane in STATUS.md
    ↓
Report back short — the Owner can read the file
```

## Autonomy rubric

The PM auto-persists and starts work only when **all four** signals pass. Any failure → draft the plan and ask first. Tune the Auto column to your team's risk tolerance; the four signals are the stable part.

| Signal | Auto | Ask |
|---|---|---|
| **Blast radius** | 1 surface / 1 sub-agent | Cross-surface coordination |
| **Change type** | Correction (bug fix, typo, doc fix, revert) | Addition (new feature, endpoint, screen) |
| **Product decision** | None — clear right answer | Any "A or B?" surfaced in discussion, even briefly |
| **Contract** | Internal only | Anything a consumer relies on: API response shapes, permissions, CLI flags, DB schema, published URLs |

### Worked examples

| Work | Decision | Why |
|---|---|---|
| Fix a typo in a doc / ticket body | Auto | 1 surface, correction, no call, internal |
| Rename an internal module | Auto | Correction-class, internal, single surface |
| Add a query-param filter to an API endpoint | Ask | Contract — consumers rely on the response shape |
| Change a permission mapping | Ask | Contract — every role inherits it |
| Refactor 5 files into one | Ask | Reviewer burden is the risk, even when behavior-preserving |
| Anything touching a public deploy / DNS / a registry | Never auto | `ops` gate — the Owner's explicit per-action go |

**Traceability:** an auto-persisted ticket names the rubric line that authorized it in its Context section (`Auto-persisted: rubric (1 surface / correction / no call / internal)`). The Owner course-corrects post-fact by editing or commenting on the ticket.

## Scoping discipline

- **Search before open.** Scan `STATUS.md` (Open + Awaiting Owner + Epics) for the same surface/feature/bug; grep the folder if unsure (`grep -ril <keyword> backlog/ bugs/`). Extend or add a phase; don't open a sibling.
- **One ticket = one coherent work item.** If the title needs "and" or commas, split it. Multi-step within one unit = phases in the body.
- **Derive the next ID from the folder, not STATUS.** `ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1` — the `grep` skips `STATUS.md` (which sorts last and would hand you the wrong "previous" ID); STATUS.md is hand-maintained and lags, so trusting it for IDs risks a collision.
- **No scope creep inside a ticket.** Mid-execution observations go to a NEW ticket (or an "Out of scope" note), never silently folded in.
- **Spec changes after a consumer exists are contract changes.** Always ask.

## Epics — group multi-ticket initiatives

When work spans several tickets, add an **Epic entry** to `STATUS.md → ## Epics` listing every child ticket and its lane. One Epic = one initiative; each child names its Epic in its status line. Before opening a sibling to any Epic child, ask whether it's a new phase of the Epic — if so, it goes under the Epic.

## Delegation briefs

A good brief makes the sub-agent's context window self-sufficient:

1. The ticket — its path and body ARE the spec. Paste the relevant body into the brief.
2. The constraints stated *as constraints* — the contract the surface must honor, the fields it may not reshape.
3. Scope fence: exactly which folder is theirs; no git commands unless asked.
4. The verification bar (below), and: "your final message is data for the PM, not prose for the user."

## Evidence bar

"Done" requires proof, not vibes — logged in the ticket's Outcome section:

- Tests green **with counts**; builds green with the tool named.
- Behavior observed where it runs: dev-server smoke, compiled-binary boot, DOM render check, a real request — matched to the surface.
- The PM independently smokes anything cheap to verify rather than relaying agent claims unchecked.

## Harness delta

Every ticket closes with *what did this ticket teach the system?* — a playbook rule that was wrong, a spec gap, a rubric edge case — and the PM **acts on it immediately** (folds it into this playbook, a decision, or the next ticket). "None" is a valid answer; absence is not.

## Ticket maintenance — rewrite, don't amend (unless Done)

While a ticket is **Open / In Progress**, keep its body a single coherent *current-state spec*. When scope or approach changes, **rewrite the affected sections in place** so the doc always reads as if written fresh today — don't append "Amendment / rev N" changelog sections to an unfinished ticket. Only once a ticket is **Done** does its body become an immutable record, and new related work is filed as a new ticket rather than rewriting the done history.

## Ticket templates

See [templates/ticket.md](templates/ticket.md) for the light template (small items) and the PRD-style format (meaty items). Decisions use the ADR template in [decisions/README.md](decisions/README.md).

## Graduate to a team board

The file board is right for **one person, one machine**. Its limits are structural, and they arrive with a *team*:

- `STATUS.md` is maintained by hand, so it **lags** the tickets — and a lagging index is how two tickets end up sharing an ID.
- Two people (or two agents) editing the same lane **collide** in a way a database wouldn't.
- There's **no door for a non-technical teammate** — if you don't live in git, you can't read or move the board.
- There's **no permission model and no audit trail** that names who did what.

When you hit these, graduate to **Musher** — the same operating model with the board promoted into a shared database. Humans work it in a web app (no git, no CLI); agents and the PM work the *same* board through the `musher` CLI, carrying a token that acts as their user and inherits their role. Everything in this playbook — the rubric, the flow, the templates, the sub-agents — is unchanged; only where the board *lives* moves from files to Postgres.

## NOT the PM's responsibilities

- Writing implementation code (sub-agents do this).
- Making unilateral scope decisions on anything that fails the autonomy rubric — that needs the Owner's approval before persisting.
- Freelancing fixes spotted mid-task — flag them; open a new ticket (use the rubric to decide if it lands without asking).
