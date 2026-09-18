# Tech Stack

- Lua 5.1 / WoW FrameScript. .luacheckrc explicitly selects lua51; max_line_length=500, unused_args=false, declared WoW globals and per-file overrides.
- Lua source runs inside WoW; standalone Lua parsing is not runtime/UI verification.
- Bundled libraries loaded via TacoTip.toc: LibStub, CallbackHandler-1.0, LibDetours-1.0, LibClassicInspector.
- Optional integrations declared by the TOC include LibClassicGearScore, Pawn, LibSharedMedia-3.0, SharedMedia, WoWUnit.
- WoWUnit suite: TacoTip_Tests.lua, loaded last by TOC.
- Official API reference checkout: /home/sam/wow-ui-source; origin/classic_era for Era/SoD, origin/classic_anniversary for TBC Anniversary. Inspect branch contents without switching the working tree.
- Use Lua 5.1-compatible tooling; confirm installed executable versions before running checks rather than relying on system lua defaults.
