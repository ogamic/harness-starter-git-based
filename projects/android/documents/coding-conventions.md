# Coding conventions

*Example stub. State the real rules; the agent treats these as law and stops on conflict.*

- **Naming:** `camelCase` members, `PascalCase` types/composables, `kebab`/`snake` resources per platform norm.
- **State:** one source of truth in the ViewModel; Composables are pure functions of state.
- **Enums/decoding:** decode API payloads defensively — an unknown value must not crash the app.
- **Strings:** no hardcoded user-facing copy if localized — add keys in every supported language.
- **Verification:** a green build is compile-proof only; runtime behavior is confirmed on a device/emulator.
