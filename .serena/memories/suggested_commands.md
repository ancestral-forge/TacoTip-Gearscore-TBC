# Suggested Commands

## Code Quality & Verification
- `luacheck .`: Run static analysis across the entire project. Must pass with 0 warnings and 0 errors.

## FrameXML API Auditing (`/home/sam/wow-ui-source`)
- `git -C /home/sam/wow-ui-source checkout origin/classic_era`: Switch to Classic Era / SoD FrameXML branch.
- `git -C /home/sam/wow-ui-source checkout origin/classic_anniversary`: Switch to TBC Anniversary FrameXML branch.
- `git -C /home/sam/wow-ui-source grep -n "<API_NAME>"`: Search official Blizzard FrameXML for exact signatures, mixins, or events.

## In-Game Addon Commands
- `/tt` or `/tacotip`: Open addon configuration options panel.
- `/tttest` or `/tacotest`: Execute WoWUnit automated test suites in-game.

## Memory & Documentation Integrity
- `serena memories check`: Run sanity check on all Serena memory references and link integrity.