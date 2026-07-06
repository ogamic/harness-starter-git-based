# Coding conventions

*Example stub. State the real rules; the agent treats these as law and stops on conflict.*

- **Naming:** `camelCase` members, `PascalCase` types/views; one primary type per file.
- **State:** one source of truth per screen in its view model; views are pure functions of state.
- **Decoding:** decode API payloads defensively — unknown enum cases fall back, they don't crash.
- **Strings:** no hardcoded user-facing copy if localized — add keys in every supported language.
- **Verification:** a green build is compile-proof only; runtime (init order, navigation) is confirmed on a device/simulator.
