# Task Completion & Verification Gates

Before claiming any task, refactoring, or feature is complete, the following gates must be verified:

1. **Static Analysis Gate:**
   - Execute `luacheck .` from repository root.
   - Result must be exactly: `0 warnings / 0 errors in 21 files`.
   - Ensure no temporary `-- luacheck: ignore` comments were introduced.

2. **Automated Unit Testing & Mock Discipline:**
   - If test cases in `TacoTip_Tests.lua` are added or touched, verify that all global monkey-patches/replaces are executed within `pcall` blocks and cleaned up via `ClearReplaces()`.

3. **Documentation & Memory Synchronization:**
   - When modifying architectural rules, config defaults, dimensions (e.g. 3D portrait `72x96`), or adding settings:
     - Update `MEMORY.md`.
     - Update relevant files in `.omg/memory/` and `.omg/rules/`.
     - Update `memory-bank/` (`activeContext.md`, `progress.md`, `systemPatterns.md`).
     - Update `README.md` and `CHANGELOG.md` if user-facing.

4. **Localization Coverage:**
   - If new text strings were added to `Locale/enUS.lua`, ensure all 10 non-English locale files define or safely inherit the keys with matching format tokens.