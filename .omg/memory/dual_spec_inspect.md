# Dual-Spec Inspection & Tooltip Formatting

## 1. Multi-Client Dual-Spec Detection

In `Libs/LibClassicInspector/LibClassicInspector.lua`:

- **Dynamic Capability Check:**

  ```lua
  local hasDualSpec = isWotlk or isTBC or (C_SpecializationInfo ~= nil) or (GetNumTalentGroups ~= nil)
  ```

- **Dynamic Event Handling:**
  `PLAYER_TALENT_UPDATE` and `ACTIVE_TALENT_GROUP_CHANGED` are registered on all dual-spec clients so in-game talent re-allocations and spec swaps trigger instant tooltip cache updates.

---

## 2. Rendering Rules in `main.lua`

- **Active vs Inactive Spec:**
  - Active spec displays on the primary talent line: `Talents: [Icon] SpecName [Points]`
  - Secondary (inactive) spec renders on the secondary line dimmed at 60% opacity: `|c99ffffff[Icon] InactiveSpec [Points]|r`
  - Dedup Guard: Inactive spec line only renders if `spec2 ~= nil` and `spec2 ~= spec1` (prevents printing the same spec twice).

- **Pixel-Perfect Vertical Alignment:**
  In compact/standard tooltip mode, the secondary line uses an invisible zero-alpha prefix matching the exact localized `L["Talents"] .. ": "` string:

  ```lua
  string.format("|c00000000%s: |r|c99ffffff%s|r", L["Talents"], specText)
  ```

  This ensures the secondary talent icon aligns horizontally with the primary talent icon across all fonts and languages without hardcoded space drift.
