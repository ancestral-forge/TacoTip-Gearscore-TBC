# Tooltip Lifecycle, Visual Isolation & 3D Portrait Alpha Sync

## 1. Non-Unit Visual Isolation (`clearTooltipVisuals`)

To ensure non-unit tooltips (items, spells, buffs, action buttons, map POIs) never inherit stale unit state:

- `clearTooltipVisuals(tooltip)` executes synchronously on:
  - `OnHide` & `OnTooltipCleared`
  - `OnTooltipSetSpell` & `OnTooltipSetItem`
  - `onTooltipShow` (when `not unit or not UnitIsPlayer(unit)`)
  - Top of `onTooltipSetUnit` (before processing new unit)

- **Purge Operations:**
  1. Increment `tooltip._borderDeferralGen` (invalidates all pending deferred timers)
  2. Cancel `borderDeferTimer` and `classBorderDeferTimer` via `cancelDeferredAppearance()`
  3. Reset player class color, border overlay, guild line, and level color line
  4. Hide 2D portrait (`tooltip.TacoTipPortrait:Hide()`)
  5. Clean 3D portrait: `tooltip.TacoTipPortrait3D:Hide()`, `pcall(tooltip.TacoTipPortrait3D.ClearModel)`, `tooltip.TacoTipPortrait3D:SetAlpha(1)`
  6. Hide and stop power bar ticker (`TacoTipPowerBar:Hide()`, `stopPowerBarTicker()`)
  7. Reset tooltip custom padding (`ClearPadding()` or `SetPadding(0, 0, 0, 0)`)
  8. Registered on GameTooltip, ShoppingTooltips, ItemRefTooltip, ItemRefShoppingTooltips, WorldMapTooltips, WorldMapCompareTooltips, and SmallTextTooltip

---

## 2. 3D Portrait Alpha Sync & Screen-Edge Detection

- **Alpha Synchronization:** In `ensureTooltipPortrait`, attach an `OnUpdate` script on `tooltip.TacoTipPortrait3D` syncing to parent alpha at 20Hz.
- **Screen-Edge Clipping Prevention:** During `ApplyTooltipAppearance`, dynamically check if `tooltipRight + portraitW + 8 > screenWidth`. If near the right edge of the screen, dynamically flip the portrait anchor from `TOPLEFT -> TOPRIGHT` to `TOPRIGHT -> TOPLEFT (-8, 0)` so the enlarged portrait never renders off-screen.
- **Lifecycle Guarantees:**
  - Zero timer drift or race conditions.
  - When `tooltip` hides, the engine halts the `OnUpdate` script automatically.
  - On new unit show, `portrait:SetAlpha(tooltip:GetAlpha() or 1)` is called before `portrait:Show()`.

---

## 3. High-Performance Zero-Allocation Hover Pipeline

- `pooledLinesToAdd`, `pooledLineRecords`, `linesToAddCount`, and `pooledPlayerText` recycle sub-tables for all tooltip text lines via `addLineDouble` and `addLineSingle`.
- Completely eliminates 38 anonymous table literals per hover and eliminates all `unpack(v)` operations in line rendering.
- `TacoTipMouseAnchor:SetScript("OnUpdate")` returns immediately when `not TacoTipConfig.anchor_mouse or not GameTooltip or not GameTooltip:IsShown()`, saving 144–240Hz cursor polling and layout calculations when tooltips are hidden.

---

## 4. Minimap & World Map Flicker Prevention

- `GameTooltip:EnableMouse(false)` is enforced inside `GameTooltip_SetDefaultAnchor` so the tooltip never intercepts mouse events and prevents `OnEnter`/`OnLeave` flicker cycles.
- Caller frame ownership is strictly preserved on `GameTooltip` (never overridden with `SetOwner(TacoTipMouseAnchor)`).
