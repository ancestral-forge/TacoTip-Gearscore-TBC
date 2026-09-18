# Task Completion Gates

- Preserve existing staged/unstaged work; inspect current git status before changes. No unsolicited commits, pushes, destructive cleanup, or branch switching.
- Syntax from project root: `for f in main.lua options.lua gearscore.lua pawn.lua textures.lua TacoTip_Tests.lua Locale/*.lua; do luac5.1 -p "$f" || exit 1; done`. Confirm Lua 5.1 compiler availability first; also parse any changed Lua files outside that list.
- Lint: `luacheck .` with zero warnings/errors.
- Runtime: execute `/tttest` with WoWUnit on the relevant supported client. Standalone syntax/lint does not prove in-game behavior; explicitly report when client testing is unavailable.
- New features require meaningful TacoTip_Tests.lua cases; reproduce regressions before fixing them where feasible.
- Behavior, defaults, UI dimensions, API, and architecture changes require synchronous README.md, CHANGELOG.md, MEMORY.md, relevant .omg/memory and .omg/rules files, and memory-bank activeContext/progress/systemPatterns updates.
- Version bumps synchronize TacoTip.toc, main.lua and options.lua version values, README.md, CHANGELOG.md, MEMORY.md, and AGENTS.md. Preserve released changelog entries.
- Inspect final diff for scope, preserved user edits, and documentation accuracy. Report commands actually executed and real exit/test results; do not claim unavailable runtime verification.
- Serena onboarding/reference maintenance: `serena memories check` from project root checks memory links; do not imply it ran without tool output.
