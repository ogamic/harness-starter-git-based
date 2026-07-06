# Gangline

**An operating system for teams of humans and coding agents.**

In dog sledding, the *gangline* is the central line that connects every dog's harness to the sled. A harness makes one dog useful; the gangline makes them a team. The AI world has spent years building better harnesses — Claude Code, Codex, Cursor, subagent packs. Gangline is the layer above: the line that connects many harnessed workers — human and AI — into one team pulling one load.

> Many harnesses. One line.

## What this is

Gangline is a **workspace-level operating model** for human + agent teams:

- **A PM agent that never codes.** One agent coordinates — it discusses intent, scopes work into tickets, and delegates execution to domain sub-agents, one per surface. It never edits source directly; each sub-agent carries its own conventions.
- **An org chart for agents.** Domain sub-agents execute; the human owner gates the decisions that are genuinely theirs. Who may act without asking is written down, versioned, and reviewable.
- **A board that's just files.** Tickets, bugs, and decisions are numbered markdown files in this repo, indexed by a `STATUS.md` you can read in any editor. No database, no service, no account — `git` is the whole backend.

It is **not** a framework you install into a single codebase, not a persona theater that role-plays an agile team, and not a subagent parts catalog. Gangline assumes your harnesses already work; it organizes them.

## This repo is the starter

This is a **template**. Use it to stand up your own workspace:

```bash
# 1. Use this template (or clone) → your own workspace repo
git clone <your-workspace> my-workspace && cd my-workspace/management
#    open your coding agent (e.g. Claude Code) HERE, in management/
```

`management/` is the hub — the PM agent's home. Start with **[management/README.md](management/README.md)** and **[management/pm-playbook.md](management/pm-playbook.md)** (the operating model: flow, autonomy rubric, evidence bar).

The board is already here — no setup:

```
management/backlog/   ← work items, one markdown file each (STATUS.md is the index)
management/bugs/      ← bugs, same shape
management/decisions/ ← ADRs: why the workspace is the way it is
```

The PM opens a ticket by writing `management/backlog/NNNN-slug.md`, updates the lane in `STATUS.md`, and delegates to a sub-agent with the ticket as the spec.

## What's in this template

```
management/   ← the hub: launch your agent here (CLAUDE.md, pm-playbook.md, board, sub-agents)
projects/     ← worked example stubs (api / app / android / ios), each documenting itself
```

`projects/api`, `projects/app`, `projects/android`, and `projects/ios` are **worked example stubs** — keep them to learn the pattern, or replace them with your own (each as its own repo). Each carries its own `documents/` (architecture, conventions, response-format) — the spec its sub-agent builds against.

## Graduate to a team board

The file board is perfect for one person and one machine. When a **team** shows up — a second human (especially a non-technical one), agents working in parallel, a need for roles/permissions or an audit trail that names who did what — a folder of markdown files starts to strain: `STATUS.md` lags behind the tickets, two people touch the same lane, and there's no door for anyone who doesn't live in git.

That's the upgrade to **Musher** — the same operating model with the board promoted into a shared database: a web app for humans (no git, no CLI) and a `musher` CLI for agents, one source of truth under one permission model. The playbook, the rubric, the templates, and the sub-agents are unchanged — only where the board *lives* changes. See [management/pm-playbook.md → "Graduate to a team board"](management/pm-playbook.md).

## License

MIT — see [LICENSE](LICENSE).
