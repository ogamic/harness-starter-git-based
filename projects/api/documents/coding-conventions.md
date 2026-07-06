# Coding conventions

*Example stub. State the real rules; the agent treats these as law and stops on conflict.*

- **Naming:** `camelCase` for variables/functions, `PascalCase` for types, `kebab-case` for files.
- **Errors:** fail loudly at the boundary; never swallow. Return typed error shapes, not bare strings.
- **Tests:** every behavior change ships with a test; report counts. No merge on red.
- **Comments:** explain *why*, not *what*. Don't narrate ticket history in source.
- **Contracts:** API response shapes and permissions are contracts — changing them is a PM-gated decision, not a local edit.
