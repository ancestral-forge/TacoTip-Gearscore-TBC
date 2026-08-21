# Ultragoal Brief: TacoTip-Gearscore-TBC Live CurseForge Addon Deep Audit

## Objective
Conduct a rigorous, full-spectrum architectural, code quality, API compatibility, localization, and release readiness audit of TacoTip-Gearscore-TBC (CurseForge Project ID: 1555962) across all supported WoW Classic client iterations (Classic Era/SoD 1.15.x, TBC Anniversary 2.5.5-2.5.6, Titanforge/Wrath 3.8.x).

## Key Audit Vectors
1. **Packaging, Metadata & TOC Integrity:**
   - Multi-client TOC header compliance (`11509`, `20506`, `38001`), Project ID, SavedVariables declaration, file load order, and distribution packaging.
2. **Core Runtime, Event System & FrameXML API Compliance:**
   - Safe tooltip hooking (`GameTooltip`, `ItemRefTooltip`, `ShoppingTooltip`), non-unit visual isolation, dynamic script hook safety, Blizzard API signatures against `/home/sam/wow-ui-source`, LibClassicInspector / LibDetours-1.0 lifecycle, and memory/performance profiling.
3. **Options UI, Settings Architecture & Persistence:**
   - Settings category registration (modern Settings Canvas API + legacy InterfaceOptions fallback), SavedVariables sanitization/migration, widget event handlers, and live UI synchronization.
4. **Localization Coverage & Fallback Integrity:**
   - `Locale/enUS.lua` truth parity across all 10 localized files (`deDE`, `esES`, `esMX`, `frFR`, `itIT`, `koKR`, `ptBR`, `ruRU`, `zhCN`, `zhTW`), string interpolation safety, and fallback behavior.
5. **Static Analysis, Test Coverage & Error Resilience:**
   - Zero-warning `luacheck` compliance, `WoWUnit` test harness verification, mock restoration security, and fail-safe pcall patterns.
6. **CurseForge Release Readiness & Synthesis:**
   - Verification against CurseForge distribution standards, documentation accuracy (`README.md`, `CHANGELOG.md`), and delivery of an exhaustive executive audit report.
