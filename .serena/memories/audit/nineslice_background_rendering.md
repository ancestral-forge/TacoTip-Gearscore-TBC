# NineSlice Background & Border Rendering Invariants

## Modern Classic FrameXML Architecture
- On Classic Era 1.15.x, TBC Anniversary 2.5.x, and Wrath/Titanforge 3.80.x, standard tooltips inherit `TooltipBackdropTemplate`.
- In `SharedTooltipTemplates.xml`, `TooltipBackdropTemplate` contains:
  `<Frame parentKey="NineSlice" inherits="NineSlicePanelTemplate" useParentLevel="true"/>`
- `TooltipBackdropTemplateMixin:SetBackdropColor(r, g, b, a)` delegates directly to:
  `self.NineSlice:SetCenterColor(r, g, b, a)`
- `TooltipBackdropTemplateMixin:SetBackdropBorderColor(r, g, b, a)` delegates to:
  `self.NineSlice:SetBorderColor(r, g, b, a)`

## Identified Codebase Defect
- TacoTip's `getOrCreateBackdropFrame(tooltip)` marks NineSlice tooltips with `isBorderOnly = true`.
- Line 685 in `main.lua` checked `if (backdrop and backdrop.SetBackdropColor and not backdrop.isBorderOnly)`.
- This bypassed background color and alpha applications on all modern NineSlice tooltips, rendering `tooltip_background_use_class`, custom background RGB, and custom background alpha non-functional on modern clients.
- `applyTooltipBorderOverlay` uses an overlay frame with a hardcoded `SetFrameLevel(2)`. Since NineSlice uses `useParentLevel="true"`, `SetFrameLevel(tooltip:GetFrameLevel() + 1)` is required to prevent rendering underneath tooltips with dynamic frame levels.

## Enterprise Solution Pattern
- Apply background colors/alpha directly via `tooltip.NineSlice:SetCenterColor(r, g, b, a)` when `tooltip.NineSlice` exists, or `backdrop:SetBackdropColor` as fallback.
- Reset background colors on `clearTooltipVisuals` using the user's default background settings.