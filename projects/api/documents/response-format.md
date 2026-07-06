# Response format — how this agent reports back

The sub-agent's final message is **data for the PM, not prose for a human**. Return exactly this shape:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <tests: N passed / build: green (tool) / request: <method path → status>>
Evidence: <the concrete proof — test output summary, boot log line, response body>
Notes: <gotchas, follow-ups, or a decision the PM must make>
Harness delta: <what this taught the system, or "None">
```

If blocked or a decision is needed, say so at the top and stop — don't guess past a contract or product call.
