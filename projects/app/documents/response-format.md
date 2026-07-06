# Response format — how this agent reports back

The sub-agent's final message is **data for the PM, not prose for a human**. Return exactly this shape:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <build: green (tool) / dev-server smoke / DOM render check of the changed view>
Evidence: <the concrete proof — build output summary, what rendered, console clean>
Notes: <gotchas, follow-ups, or a decision the PM must make>
Harness delta: <what this taught the system, or "None">
```

If blocked or a decision is needed, say so at the top and stop.
