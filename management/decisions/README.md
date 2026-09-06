# ADRs — Architecture Decision Records

One file per decision, numbered `NNNN-kebab-case-title.md`. These record *why* the workspace and its operating model are the way they are — the durable "why not the other way" that would otherwise be lost in chat.

Owner-governed. Anything cross-cutting — operating model, agent/workflow conventions, permissions, tooling policy — belongs here. Tactical work items go to `../backlog/`; this folder is for decisions.

**Numbering:** `0000` is the retroactive baseline capturing pre-existing principles. Forward decisions increment from `0001`. When a decision is reversed, mark the old ADR `Superseded by NNNN` rather than deleting it.

**Pending / unmade decisions** live in [CANDIDATES.md](CANDIDATES.md).

## Template

```markdown
# ADR NNNN: <title>

**Status:** Proposed | Accepted | Superseded by <ADR-NNNN>
**Date:** YYYY-MM-DD
**Owner:** <the Owner>

## Context
What situation forced this decision. What's non-obvious. What constraints matter.

## Decision
What we're doing. One or two sentences, no hedging.

## Alternatives considered
- **<alt>:** why not

## Consequences
What becomes easier. What becomes harder. What we'll need to watch.
```

## Index

| ADR | Decision | Date |
|---|---|---|
| [0000](0000-baseline-operating-principles.md) | Baseline — PM never codes, file board, kebab/ALL_CAPS naming | 2026-01-01 |
| [0001](0001-multi-harness-agent-definitions.md) | One agent definition, three harnesses — `agents/` + a generator | 2026-09-06 |
