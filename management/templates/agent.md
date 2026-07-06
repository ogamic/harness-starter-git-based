# Sub-agent template

Copy into `.claude/agents/<name>.md`. A sub-agent definition is a **slim index** — its domain, its stack, its conventions, and (critically) what it must *not* touch. Keep it short; point at the sub-project's `documents/` for detail rather than duplicating it.

```markdown
---
name: <surface>            # e.g. api, app, android
description: Use this agent for <surface> — <one-line domain + stack>. Do NOT use for <the other surfaces>.
---

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
