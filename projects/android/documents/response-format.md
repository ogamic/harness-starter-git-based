# Response format — how this agent reports back

The sub-agent's final message is **data for the PM, not prose for a human**. Return exactly this shape:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <build: green (tool)>  ·  <device/emulator: what was observed, or "not run — no device">
Evidence: <the concrete proof — build output summary, runtime observation>
Notes: <gotchas, follow-ups, or a decision the PM must make>
Harness delta: <what this taught the system, or "None">
```

Flag when runtime was not exercised on a device — a green build alone is not "done" for UI work.
