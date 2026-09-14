# TacoTip-Gearscore-TBC — Core

Dual-engine World of Warcraft Classic addon providing tooltip enhancements, GearScore calculation, talent and specialization inspection (including dual-spec), Pawn integration, class-colored backdrops/borders, and 3D character portraits.

## Load Order & Source Map (TOC Order)
1. Libs: `Libs/LibStub/`, `Libs/CallbackHandler-1.0/`, `Libs/LibClassicInspector/` (talents/dual-spec), `Libs/LibDetours-1.0/`
2. Localization: `Locale/enUS.lua` (source of truth) and 10 regional translations (`deDE`, `esES`, `esMX`, `frFR`, `itIT`, `koKR`, `ptBR`, `ruRU`, `zhCN`, `zhTW`)
3. `gearscore.lua`: GearScore calculation engine and inspect history cache (`TT_GS`, `TacoTipGSHistory`)
4. `pawn.lua`: Pawn scale score calculation bridge (`TT_PAWN`)
5. `textures.lua`: Built-in textures and SharedMedia registration
6. `options.lua`: Settings UI (modern Settings Canvas + legacy InterfaceOptions fallback, media selectors, live preview)
7. `main.lua`: Core addon initialization (`TT`), hooks, GameTooltip styling, 3D portraits, mover frames, unit events
8. `TacoTip_Tests.lua`: In-game WoWUnit test suite (`/tttest`)

## Shared Globals & Invariants
- `_G.TT` (`_G["TacoTip"]`): Core addon table. Do not introduce new globals; namespace helper functions under `TT`.
- `_G.TT_GS`: GearScore logic and item score calculations.
- `_G.TT_PAWN`: Pawn calculation bridge.
- `_G.TacoTipConfig`: SavedVariables table storing user configurations.
- `_G.TACOTIP_LOCALE`: Active localization table.

## Related Memories
- For runtime environment, supported interface IDs, and library dependencies, read `mem:tech_stack`.
- For code style, zero-allocation buffers, API discipline, and non-unit isolation rules, read `mem:conventions`.
- For essential terminal commands, FrameXML research paths, and in-game slash commands, read `mem:suggested_commands`.
- For mandatory verification gates and checklists before completing any task, read `mem:task_completion`.