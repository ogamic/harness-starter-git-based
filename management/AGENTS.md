# AGENTS.md

**Launch your coding agent from this folder (`management/`).** This is the hub of a Gangline workspace — the operating layer. Your code lives in sibling folders under `../projects/`.

## Core rule

The main agent here is the **PM / chief of staff** — it discusses intent, scopes work, delegates execution, and logs outcomes. It **never writes implementation code** — sub-agents do, one per surface (defined in `agents/`). See [pm-playbook.md](pm-playbook.md) for the full flow, autonomy rubric, delegation briefs, and evidence bar.

## The board is files in this repo

This workspace is **file-native**. The board of record — tickets, bugs, decisions — lives as numbered markdown files here in `management/`, indexed by `STATUS.md`. `git` is the whole backend; there is no database and no service to run.

```
backlog/NNNN-slug.md     # one work item per file
backlog/STATUS.md        # the board: lanes (Open / Awaiting Owner / Epics / Closed)
bugs/NNNN-slug.md        # one bug per file
bugs/STATUS.md
decisions/NNNN-slug.md   # ADRs — why the workspace is the way it is
decisions/README.md      # ADR template + index
decisions/CANDIDATES.md  # pending / unmade decisions
```

The PM opens a ticket by writing a file, records its lane in the matching `STATUS.md`, and delegates to a sub-agent with the **file path as the spec**. See [pm-playbook.md](pm-playbook.md) for the ticket templates and the search-before-open rule.

## Roles

- **Owner** — the accountable human. Gates product decisions, decision acceptance, contract changes, and every irreversible edge.
- **PM agent** — the main agent, launched from `management/`. Discusses intent, scopes, persists tickets to `backlog/`/`bugs/`, delegates, logs outcomes. **Never writes implementation code.**
- **Sub-agents** (`agents/`): one per surface. `ops` is the only agent at the public edge (releases/deploys), hard-gated on the Owner.

## Harnesses — one definition, three tools

Roles and skills are defined **once**, harness-neutral, and fanned out to Claude Code, Codex, and Antigravity by a generator. Nothing here is Claude-specific.

```
SOURCE — hand-edit these          GENERATED — never hand-edit, but do commit
AGENTS.md                         .claude/agents/<name>.md      Claude Code subagents
CLAUDE.md  (imports AGENTS.md)    .codex/agents/<name>.toml     Codex custom agents
agents/manifest.json              .agents/skills/<name>/SKILL.md  Antigravity (roles → skills)
agents/<name>.md                  .claude/skills/<n>/SKILL.md
skills/<name>/SKILL.md            .codex/skills/<n>/SKILL.md
scripts/sync-agents.mjs           .agents/skills/<n>/SKILL.md
```

Rules:

- **Never hand-edit `.claude/`, `.codex/`, or `.agents/`.** Every file there carries a GENERATED banner and is overwritten. Edit the source, run `node scripts/sync-agents.mjs`, and commit **both** the source and the regenerated output.
- `node scripts/sync-agents.mjs --check` exits non-zero when the generated files are stale — wire it into CI or a pre-commit hook.
- **Codex and Antigravity read `AGENTS.md` natively.** Claude Code does not, so `CLAUDE.md` is a one-line import of it.
- **Antigravity has no file-based subagent primitive**, so each role is emitted as a *skill* (`/name` invocation plus semantic discovery). It also caps every rules/skill file at **12,000 characters** — keep `AGENTS.md` and each role prompt well under it; if one grows, split the detail into a linked doc rather than truncating meaning. The generator fails loudly rather than emitting an oversized file.
- **Lane discipline.** Two harnesses editing one repo at once will conflict. Give each tool its own project, or its own `git worktree`.
- **Guardrails are prompt-level, not harness-level.** Claude Code hooks (`.claude/hooks/*`) have no equivalent event in Codex or Antigravity, and permission deny-lists are Claude-only (Codex has `sandbox_mode`; Antigravity has nothing). Under the other harnesses those checks are run by hand. The `ops` agent's "never" list and the Owner's explicit per-action go are the real protection.
- **No harness-private memory.** Durable knowledge does not go in `~/.claude`, `~/.codex`, or `~/.gemini` — un-versioned, invisible to the other two, and it drifts silently. It goes in the board.

## Surfaces & sub-agents

Each sub-project is a **stub** slot you flesh out, carrying its own docs in `<sub-project>/documents/`:

| Sub-project | Folder | Agent | Example stack |
|---|---|---|---|
| Backend service | `../projects/api` | `api` | Node · Express · Postgres |
| Web client | `../projects/app` | `app` | React · Vite · Tailwind |
| Android client | `../projects/android` | `android` | Kotlin · Compose · MVVM |
| iOS client | `../projects/ios` | `ios` | Swift · SwiftUI · async/await |

Adding a real sub-project = drop code into the folder, fill in its `documents/`, write the role body at `agents/<name>.md`, register it in `agents/manifest.json`, run `node scripts/sync-agents.mjs`, and commit the generated output (see [templates/agent.md](templates/agent.md)).

## Rules of the road

- **The board is files.** Track work as markdown in `backlog/`/`bugs/`; keep `STATUS.md` current in the same change.
- **Decisions live in `decisions/`** as ADRs — record the choice *and its rationale*; supersede rather than rewrite.
- **A sub-project's `documents/` is its law** — the owning agent reads it first and stops on any conflict rather than improvising.
- **The PM never writes implementation code** — every code change goes to the owning surface agent.

When your team outgrows a file board, see [pm-playbook.md → "Graduate to a team board"](pm-playbook.md).
