# Skills

Packaged procedures the PM agent invokes by name. These are **not** conveniences — each one exists to enforce a rule from [pm-playbook.md](../../pm-playbook.md) that's easy to skip when you're moving fast.

**A skill earns a slot only if it encodes a rule people (and agents) reliably get wrong.** A wrapper around something the playbook already makes obvious is bloat — Gangline is an operating model, not a parts catalog. When you add a skill, name the playbook rule it protects; if you can't, it doesn't belong here.

## Tier 1 — the operating loop, made self-enforcing

| Skill | Protects the rule | 
|---|---|
| [`ingest`](ingest/SKILL.md) | Onboarding existing docs → the board shape, **classify-then-confirm** (never bulk-import blind) |
| [`ticket-new`](ticket-new/SKILL.md) | Search-before-open · **ID from the folder, not STATUS** · run the 4-signal rubric before persisting |
| [`ticket-close`](ticket-close/SKILL.md) | Evidence bar *with counts* · the **harness-delta** step everyone forgets |
| [`board-doctor`](board-doctor/SKILL.md) | The file board's structural weakness: `STATUS.md` lags by hand — this reconciles it |

## How skills invoke

The PM runs a skill when the situation matches its description. A skill is a checklist the agent *executes*, not a doc it reads — it ends by reporting what it did (or what it stopped on) as data, per the delegation-brief contract.
