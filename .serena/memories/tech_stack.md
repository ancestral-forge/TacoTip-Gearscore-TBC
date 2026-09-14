# Tech Stack & Environment

## Languages & Dialects
- **Lua 5.1**: Target runtime dialect for World of Warcraft Classic FrameXML.

## Supported WoW Client Interfaces
- `11509`: WoW Classic Era 1.15.x / Season of Discovery
- `20506`: WoW TBC Classic Anniversary 2.5.6
- `38001`: Titanforge Chinese Wrath client
- Note: `30405` is deprecated. Retail (`WOW_PROJECT_MAINLINE`) features must be isolated behind project ID checks or omitted.

## Bundled & Optional Libraries
- `LibStub`: Library version registration.
- `CallbackHandler-1.0`: Event/callback dispatcher.
- `LibClassicInspector`: Talent scanning and dual-spec query resolution across Classic Era, SoD, and TBC Anniversary.
- `LibDetours-1.0`: Function hook detour engine.
- `LibSharedMedia-3.0`: Optional runtime media integration (statusbar textures, fonts, borders, backgrounds).

## Static Analysis & Testing
- `luacheck`: Static analyzer enforcing strict zero-warning policy (`.luacheckrc`, `std = "lua51"`).
- `WoWUnit`: In-game test suite framework (`TacoTip_Tests.lua`).

## Blizzard API Reference
- Local FrameXML repository: `/home/sam/wow-ui-source`
- Active branches: `origin/classic_era` (Era/SoD) and `origin/classic_anniversary` (TBC Anniversary).