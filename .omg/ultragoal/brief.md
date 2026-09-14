# Ultragoal Brief: Deep Addon Audit, 3D Portrait Resizing & Performance Optimization

## Objective

Perform an enterprise-grade deep audit of TacoTip-Gearscore-TBC across Classic Era, SoD, TBC Anniversary, and Titanforge clients against the official Blizzard FrameXML sources at `/home/sam/wow-ui-source`. Increase the 3D portrait base dimensions while strictly preserving the 3:4 aspect ratio, eliminate major memory allocation bottlenecks (GC churn) and redundant C-API lookups, harden event registrations against raid combat performance degradation, and maintain 100% zero-warning static analysis compliance and comprehensive documentation synchronization.

## Scope & Constraints

1. **3D Portrait Resizing (3:4 Aspect Ratio):**
   - Increase base portrait dimensions from `60x80` to `72x96` (20% increase, exact 3:4 ratio: `72/96 = 0.75`).
   - Sizing at standard scale increments yields clean integer pixel dimensions (50% = 36x48, 100% = 72x96, 150% = 108x144, 200% = 144x192).
   - Update `Portrait:DefaultSizeIs34Ratio` and `Portrait:ScaledSizeKeepsRatio` unit tests in `TacoTip_Tests.lua`.
2. **High-Frequency Performance & Memory Optimizations:**
   - **SharedMedia Resolution Caching:** Stop rebuilding, preview-formatting, and sorting all SharedMedia assets (`GetTooltipBackgroundChoices`, `GetTooltipBorderChoices`, `GetTooltipFontChoices`, `GetTooltipStatusBarTextureChoices`) on every single unit hover in `options.lua`. Cache resolved media paths and invalidate only on config change or `LibSharedMedia_Registered` events.
   - **Tooltip Text Line Pooling:** Implement a table pool for `linesToAdd` row allocations in `main.lua` to eliminate garbage collection pressure during rapid mouseover churn.
   - **GearScore ItemInfo Optimization:** Eliminate `{ GetItemInfo(ItemLink) }` table wrapping in `gearscore.lua` and avoid duplicate C-API `GetItemInfo` queries for equipped weapons.
3. **Event & Lifecycle Hardening:**
   - **TacoTipPowerBar Event Gating:** Ensure `TacoTipPowerBar:OnEvent` does not execute `resolveTooltipUnit` during heavy combat when the bar is hidden; unregister events when hidden and utilize `RegisterUnitEvent` when shown.
   - **Deduplicate Tooltip Hooks:** Remove legacy redundant `OnTooltipCleared` and `OnHide` hooks on `GameTooltip` (lines 1709-1717) that duplicate `registerTooltipVisualClearing`.
   - **UNIT_TARGET Gating:** Add fast early-return when `GameTooltip` is hidden or `show_target` is disabled.
   - **3D Portrait OnUpdate Optimization:** Replace anonymous closure recreation with a static handler throttled to 20Hz (0.05s) using `self:GetParent():GetAlpha()`.
4. **Governance & Memory Integrity:**
   - Zero-warning `luacheck .` compliance across all 21 files.
   - Synchronize all real parameters (`72x96` portrait dimensions, memory architectures, version notes) across `MEMORY.md`, `AGENTS.md`, `.omg/memory/`, `memory-bank/`, `README.md`, and `CHANGELOG.md`.
