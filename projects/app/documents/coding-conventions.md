# Coding conventions

*Example stub. State the real rules; the agent treats these as law and stops on conflict.*

- **Components:** one component per file, `PascalCase`; keep them small and focused.
- **State:** lift only when shared; avoid a global store for local concerns.
- **Styling:** design tokens over literals; no inline magic numbers where a token exists.
- **Strings:** no hardcoded user-facing copy if the project is localized — add keys in every supported language.
- **Tests:** cover view logic and data wiring; report counts. No merge on red.
