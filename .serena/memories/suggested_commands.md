# Suggested Commands

- Syntax gate from project root: `for f in main.lua options.lua gearscore.lua pawn.lua textures.lua TacoTip_Tests.lua Locale/*.lua; do luac5.1 -p "$f" || exit 1; done` (requires installed Lua 5.1 compiler).
- Static analysis: `luacheck .`; target zero warnings/errors with project .luacheckrc.
- In-game options: `/tt` or `/tacotip`.
- In-game WoWUnit tests: `/tttest` or `/tacotest`.
- API signature research without branch switching: `git -C /home/sam/wow-ui-source grep -n '<API_NAME>' origin/classic_era --` or substitute origin/classic_anniversary.
- Inspect reference source: `git -C /home/sam/wow-ui-source show origin/classic_anniversary:<path>`.
- Serena memory link check from project root: `serena memories check`.

Commands are workflow guidance, not evidence that checks have run. Report actual results and unavailable tooling explicitly.
