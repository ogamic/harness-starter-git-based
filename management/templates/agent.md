# Sub-agent template

A sub-agent definition is a **slim index** — its domain, its stack, its conventions, and (critically) what it must *not* touch. Keep it short; point at the sub-project's `documents/` for detail rather than duplicating it.

Definitions are **harness-neutral**: the body lives in `agents/<name>.md` with **no YAML frontmatter**, its routing metadata lives in `agents/manifest.json`, and `scripts/sync-agents.mjs` fans both out to Claude Code, Codex, and Antigravity. Never write into `.claude/`, `.codex/`, or `.agents/` by hand — the generator overwrites them.

## Adding an agent

1. **Write the body** at `agents/<name>.md` — prose only, no frontmatter (the shape below).
2. **Register it** in `agents/manifest.json` — the `description` is what every harness uses to route work to this agent, so it belongs there, not in the body.
3. **Generate:** `node scripts/sync-agents.mjs`
4. **Commit both** the source and the regenerated `.claude/` `.codex/` `.agents/` output. `node scripts/sync-agents.mjs --check` exits non-zero if they've drifted.

Renaming or deleting an agent = rename/delete `agents/<name>.md`, update the manifest, re-run the generator (it prunes the orphaned generated files for you).

## The manifest entry

```json
{
  "name": "<surface>",
  "description": "Use this agent for <surface> — <one-line domain + stack>. Do NOT use for <the other surfaces>.",
  "claude": { "tools": "Read, Write, Edit, Glob, Grep, Bash", "model": "opus" },
  "codex": { "model_reasoning_effort": "high", "sandbox_mode": "workspace-write" }
}
```

- `claude.tools` / `claude.model` are **optional** — omit either key to inherit the harness default. Don't pin a tools list you don't actually need.
- `codex.sandbox_mode` is `workspace-write` for every surface agent. Only `ops` gets `danger-full-access`, because it works the deploy edge.
- Antigravity has no per-agent knobs — the role is emitted as a skill and reads `AGENTS.md` for the shared contract. Keep each body **under 12,000 characters**; the generator refuses to emit a file Antigravity would truncate.

## The body — `agents/<name>.md`

```markdown
You own **`../projects/<surface>`** and nothing else.

## Your surface
- Stack: <language · framework · datastore>
- Structure & conventions: read `../projects/<surface>/documents/` FIRST — it is your law. Stop and flag any conflict with the ticket rather than improvising.

## How you work
- The PM hands you a ticket file path + body — that IS your spec. Build to it; don't expand scope.
- Report back in the shape defined by `../projects/<surface>/documents/response-format.md`. Your final message is **data for the PM, not prose for a human**.
- Verification bar: tests green with counts, build green with the tool named, behavior observed where it runs.

## Scope fence
- Touch only `../projects/<surface>`. No changes to other surfaces, the hub, or the board.
- No git commands unless the PM explicitly asks.
```
