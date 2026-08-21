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
  7. Reset tooltip custom padding

---

## 2. 3D Portrait Real-Time Alpha Synchronization

- **Problem:** WoW `PlayerModel` frames render geometry in a separate pass and do not automatically inherit parent alpha during `GameTooltip:FadeOut()`.
- **Solution:** In `ensureTooltipPortrait`, attach an `OnUpdate` script on `tooltip.TacoTipPortrait3D`:

  ```lua
  tooltip.TacoTipPortrait3D:SetScript("OnUpdate", function(self)
      if (tooltip and tooltip.GetAlpha) then
          local a = tooltip:GetAlpha()
          if (self:GetAlpha() ~= a) then
              self:SetAlpha(a)
          end
      end
  end)
  ```

- **Lifecycle Guarantees:**
  - Zero timer drift or race conditions.
  - When `tooltip` hides, the engine halts the `OnUpdate` script automatically.
  - On new unit show, `portrait:SetAlpha(tooltip:GetAlpha() or 1)` is called before `portrait:Show()`.

---

## 3. Minimap & World Map Flicker Prevention

- `GameTooltip:EnableMouse(false)` is enforced inside `GameTooltip_SetDefaultAnchor` so the tooltip never intercepts mouse events and prevents `OnEnter`/`OnLeave` flicker cycles.
- Caller frame ownership is strictly preserved on `GameTooltip` (never overridden with `SetOwner(TacoTipMouseAnchor)`).
