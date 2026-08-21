# Options UI System & Settings Management

## 1. Dual Canvas & Legacy Settings Registration

TacoTip supports both modern Dragonflight/Classic settings architecture and legacy InterfaceOptions:

- **Modern Settings API:**
  `Settings.RegisterCanvasLayoutCategory(rootFrame, title)` and `Settings.RegisterCanvasLayoutSubcategory(parentCat, childFrame, title)`.
- **Legacy Fallback:**
  `InterfaceOptions_AddCategory(frame)` with `childFrame.parent = rootFrame.name`.
- **Double-Open Workaround:**
  Handles legacy Blizzard canvas bugs where child frames could open with 0 size on initial click by forcing layout refreshes.

---

## 2. Options Architecture

- **Categories:**
  1. `TacoTip Gearscore TBC` (Root/General: language selector, client toggles, GS format, reset)
  2. `Tooltips` (Styles, font/bar/backdrop/border media selectors, 3D portrait, alpha/color swatches)
  3. `Positioning` (Mover anchor selection, custom position coordinates, mouse anchoring)
  4. `Character & Inspect` (Character/Inspect frame GS & iLvl overlays and X/Y offset sliders)

---

## 3. Custom Media Dropdown Selector

- Implemented as a fixed-height scrollable modal frame (`TacoTipMediaPickerFrame`).
- Displays live statusbar texture strip previews, real 9-slice sliced borders, and custom scrollbar.
- Supports ESC key and click-outside auto-dismissal.

---

## 4. Configuration Sanitization

`SafeSanitizeConfig(cfg)` validates all config entries:

- Coerces boolean keys.
- Clamps numeric bounds (font size `8..32`, portrait zoom `0.1..2.0`, alpha `0..1`).
- Ensures fallback defaults for any nil keys.
