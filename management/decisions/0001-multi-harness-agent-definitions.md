# ADR 0001: One agent definition, three harnesses

**Status:** Accepted
**Date:** 2026-09-06
**Owner:** <the Owner>

## Context

Gangline's premise is *many harnesses, one line* — the operating model sits **above** any one coding agent. The starter contradicted that: every role and skill was written as a Claude Code artifact (`.claude/agents/<name>.md` with YAML frontmatter, `.claude/skills/`), so a team on Codex or Antigravity got a workspace with a board and a playbook but no agents.

The naive fix — hand-maintain a Claude copy, a Codex copy, and an Antigravity copy of each role — fails on contact. The three harnesses want genuinely different file formats (markdown + YAML frontmatter, TOML, `SKILL.md`), in different directories, and they do not agree on what an agent even *is*. Three hand-written copies of the same prompt drift within a week, and the drift is silent: you only discover it when the Codex `ops` agent is missing the "never" clause the Claude one grew.

Harness capabilities also differ in ways that matter for guardrails. Claude Code has PostToolUse hooks and permission deny-lists; Codex has a single `sandbox_mode`; Antigravity has neither, no file-based subagent primitive at all, and a hard 12,000-character cap on any rules or skill file.

## Decision

Roles and skills are defined **once, harness-neutral**, and generated into each harness's format by `scripts/sync-agents.mjs`.

1. **Source of truth:** `AGENTS.md` (the shared contract), `agents/manifest.json` (per-role routing + per-harness knobs), `agents/<name>.md` (the role prompt, no frontmatter), `skills/<name>/SKILL.md`.
2. **Generated and committed, never hand-edited:** `.claude/agents/*.md`, `.codex/agents/*.toml`, `.agents/skills/*/SKILL.md`, and verbatim skill copies under all three. Each carries a GENERATED banner; `--check` exits non-zero when they're stale.
3. **`CLAUDE.md` is a one-line import of `AGENTS.md`.** Codex and Antigravity read `AGENTS.md` natively; Claude Code doesn't, so the shim exists solely to import it. Claude-only notes go below that line — nothing else.
4. **Guardrails move to prompt level.** The `ops` agent's "never" list and the Owner's explicit per-action go are the protection, not harness config.

## Alternatives considered

- **Hand-maintain three configs.** Rejected: the copies drift silently, and the drift lands hardest on `ops` — the one agent whose prompt is a safety boundary. A generator makes divergence impossible rather than merely discouraged.
- **Pick one harness and say so.** Honest, but it makes Gangline a Claude Code accessory. The whole claim is that the operating model outlives any single harness; shipping Claude-only config was the gap this decision closes.
- **A runtime adapter / shim that translates on the fly.** More moving parts, a dependency to install, and nothing to read in a diff. A build step whose output is committed keeps every harness's config reviewable in git — you can see exactly what Codex will load.
- **Port Claude's hooks and deny-lists to the other harnesses.** There is nothing to port them *to*. Emulating a PostToolUse event by wrapping tool calls would be inventing a harness feature, which is out of scope for a starter template.
- **Antigravity subagent files.** No such primitive exists — `agy agents` reads a server-side list, not the repo. Roles are therefore emitted as **skills** (`.agents/skills/<name>/SKILL.md`), which gives both `/name` invocation and semantic auto-discovery, and each one points back at `AGENTS.md` for the shared contract.

## Consequences

- **One edit, three harnesses.** Change a role body or a skill, run `node scripts/sync-agents.mjs`, commit both. Adding a sub-project agent is a manifest entry plus a prompt file.
- **A build step now exists** in a workspace that previously had none. Forgetting it ships stale config, so `--check` is the guard — wire it into CI or a pre-commit hook.
- **Generated directories are committed**, which makes some diffs noisier. That's the price of every harness loading its config straight from the repo with no install step.
- **Hooks and permission deny-lists deliberately do not port.** Under Codex or Antigravity those checks are run by hand. Any guardrail that must hold everywhere has to live in a prompt or in the Owner's gate — never in harness config.
- **The 12,000-character Antigravity cap is now a hard constraint** on `AGENTS.md` and on every role prompt. The generator throws rather than emitting a file Antigravity would silently truncate, so the cap surfaces at sync time instead of as mysteriously missing instructions mid-task.
- **Lane discipline becomes an operating rule.** Two harnesses editing one repo at once will conflict; each tool gets its own project or its own `git worktree`.
- **No harness-private memory.** Durable knowledge stays on the board — `~/.claude`, `~/.codex`, and `~/.gemini` are un-versioned and invisible to the other two.
