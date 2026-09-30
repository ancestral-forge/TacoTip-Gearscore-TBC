# Ultragoal Brief: 1:1 Port of TacoTip to WoW Forever and Retail (TacoTip_Forever)

## Objective

Create a complete, 1:1 drop-in port of TacoTip in `TacoTip_Forever/` targeting WoW Forever (`1.60.x`) and Retail/Live (`11.x/12.x`), adapting all WoW Classic FrameXML APIs to modern Retail/Forever equivalents (`TooltipDataProcessor`, modern `Settings` canvas layout, modern inspect/specialization APIs, item level & scoring) while preserving exact UI layout, visual features, slash commands (`/taco`, `/tacotip`), locale tables, and zero-warning static analysis standards.

## Architecture & Porting Scope

1. **TOC & Packaging (`TacoTip_Forever.toc`):**
   - Configure for modern Interface versions (Forever `16001`, Retail `110000`, `120000`).
   - Include Libs, Locales, core modules (`gearscore.lua`, `pawn.lua`, `textures.lua`, `options.lua`, `main.lua`).
2. **Libraries & Locale Sync (`TacoTip_Forever/Libs`, `TacoTip_Forever/Locale`):**
   - Port `LibStub`, `CallbackHandler-1.0`, `LibDetours-1.0`, and adapt `LibClassicInspector` or modernize inspection for Retail/Forever.
   - Synchronize all 11 locale files (`enUS`, `deDE`, `esES`, `esMX`, `frFR`, `itIT`, `koKR`, `ptBR`, `ruRU`, `zhCN`, `zhTW`).
3. **Core Tooltip Pipeline Adaptation (`main.lua`):**
   - Replace or augment `OnTooltipSetUnit` with `TooltipDataProcessor.AddTooltipPostCall(Enum.TooltipDataType.Unit, ...)` for modern Retail/Forever tooltip lifecycle.
   - Maintain unit resolution via `TooltipUtil.GetDisplayedUnit(tooltip)` and fallback GUID checks.
   - Preserve 3D portraits (72x96 3:4 ratio), status bars (health, power), class borders, and mover positioning.
4. **Options & Settings UI (`options.lua`):**
   - Modern `Settings.RegisterCanvasLayoutCategory` / `Settings.RegisterAddOnCategory` registration.
   - Retain slash commands `/taco`, `/tacotip`, `/tip`, `/tt`, `/gs`, `/gearscore`.
5. **GearScore, Pawn & Stats (`gearscore.lua`, `pawn.lua`, `textures.lua`):**
   - Adapt version gates from Classic (`clientBuildMajor` 1-3) to support modern client builds (Forever & Retail).
   - Retain scoring logic, item level calculation, and Pawn integration.
6. **Validation & Quality Gates:**
   - Full static analysis verification with `luacheck` ensuring zero warnings/errors.
   - Unit test suite coverage.
