---
description: Tooltip visual isolation and synchronous visual clearing rules
globs: ["main.lua"]
alwaysApply: true
---

# Rule 03: Non-Unit Visual Isolation & Reset Safety

- **Synchronous Clearing:** `clearTooltipVisuals(tooltip)` must execute synchronously on every show/clear transition (`OnHide`, `OnTooltipCleared`, `OnTooltipSetSpell`, `OnTooltipSetItem`, and `onTooltipShow` for non-units).
- **Nil Safety:** Always ensure `clearTooltipVisuals` performs safe checks (`tooltip.GetName and tooltip:GetName()`).
- **GPU Mesh Purging:** Whenever hiding `tooltip.TacoTipPortrait3D`, call `pcall(tooltip.TacoTipPortrait3D.ClearModel)` and reset `SetAlpha(1)`.
- **Generation Invalidation:** Always increment `tooltip._borderDeferralGen` on clear transitions to invalidate pending `C_Timer.NewTimer` callbacks.
- **Mouse Disablement:** Enforce `tooltip:EnableMouse(false)` in `GameTooltip_SetDefaultAnchor` to prevent mouse capture loops with map and action bar frames.
