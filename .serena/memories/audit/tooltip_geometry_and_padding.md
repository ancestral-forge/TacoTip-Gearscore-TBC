# Tooltip Geometry, Padding & Anchor Invariants

## Portrait Off-Screen Clipping
- `main.lua:738` unconditionally anchors the 3D/2D portrait frame to `TOPLEFT` of portrait to `TOPRIGHT` of tooltip (`8, 0`).
- When the tooltip is at the default bottom-right position (or near the right screen edge), the portrait renders off-screen beyond `UIParent:GetRight()`.
- Solution: Dynamic smart anchoring:
  If `(tooltip:GetRight() or 0) + portraitW + 8 > (UIParent:GetRight() or GetScreenWidth())` then
      portrait:SetPoint("TOPRIGHT", tooltip, "TOPLEFT", -8, 0)
  else
      portrait:SetPoint("TOPLEFT", tooltip, "TOPRIGHT", 8, 0)
  end

## Bottom Padding Cleanup Fallback
- `main.lua:1446` calls `tooltip:SetPadding(0, 10, 0, 0)` when the power bar is shown.
- Line 852 only cleared padding if `tooltip.ClearPadding` exists.
- In WoW Classic FrameXML (`SharedTooltipTemplates.lua`), `ClearPadding` is often missing on GameTooltip; Blizzard's official fallback is:
  `if self.ClearPadding then self:ClearPadding() elseif self.SetPadding then self:SetPadding(0, 0, 0, 0) end`
- Without this fallback, non-unit tooltips (items, spells) retain an orphaned 10px bottom gap after hovering a unit.

## Extended Tooltip Coverage for Visual Isolation
- Comparison frames (`ItemRefShoppingTooltip1`, `ItemRefShoppingTooltip2`, `WorldMapCompareTooltip1`, `WorldMapCompareTooltip2`) must be registered in `registerTooltipVisualClearing` to ensure complete immunity from border/backdrop bleed-through.