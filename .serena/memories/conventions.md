# Code Conventions & Invariants

## Zero-Warning Luacheck Policy
- Never use `-- luacheck: ignore` comments in production code.
- Avoid direct assignments to external Blizzard globals or `GameTooltip` methods if flagged by luacheck; use dynamic table indexing (e.g. `GameTooltip["Method"]` or `_G["Name"]`).

## FrameXML API Discipline
- Always verify API signatures against `/home/sam/wow-ui-source` before writing Blizzard API calls.
- Never use retail-only APIs without feature-detection or `WOW_PROJECT_ID` branching. Handle legacy vs modern differences gracefully (e.g. `C_SpecializationInfo`, `C_AddOns.GetAddOnMetadata` vs `GetAddOnMetadata`).

## Non-Unit Visual Isolation
- `clearTooltipVisuals(tooltip)` must be nil-safe (`tooltip.GetName and tooltip:GetName()`) and immediately hide all unit-specific overlays (portraits, 3D models, elite dragon textures, power bars) and reset backdrop/border states on non-unit hovers (items, spells, bags, map POIs).

## Zero-Allocation High-Frequency Pipeline
- Use static pooled tables (`pooledLinesToAdd`, `pooledTooltipText`) in `main.lua` to avoid creating garbage during frequent mouseovers.
- Cache SharedMedia resolutions via `TT:InvalidateResolvedMediaCache()` in `options.lua`.
- Model frame synchronization in 3D portrait frames must be throttled to 20Hz (0.05s) using parent alpha.

## Tooltip Text & Formatting Rules
- Always wrap static label prefixes in explicit inline color codes (e.g. `|cFFFFFFFFLevel|r`) to avoid Blizzard's default gold font color.
- Friendly player level numbers must be formatted in pure white (`|cFFFFFFFF<Level>|r`). Difficulty color (`getHostileDifficultyColor`) is strictly reserved for hostile/attackable units (`UnitCanAttack("player", unit)`).

## Localization Rules
- `Locale/enUS.lua` is the definitive source of truth. Add new string keys to `enUS.lua` first.
- Maintain 100% key parity across all 11 locale files (currently 261 keys) with exact format specifier match (`%s`, `%d`).

## Version Bump Protocol
- When releasing a new version, bump simultaneously across: `TacoTip.toc`, `main.lua` (`addOnVersion`), `options.lua`, `README.md`, `CHANGELOG.md`, `AGENTS.md`, and `MEMORY.md`.