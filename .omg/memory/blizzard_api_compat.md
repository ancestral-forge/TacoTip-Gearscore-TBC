# Blizzard FrameXML API Compatibility & Research Protocols

## 1. FrameXML Source Mapping

All WoW API research must reference the local Blizzard FrameXML repository at `/home/sam/wow-ui-source`:

| Target Client Flavor | Branch in `/home/sam/wow-ui-source` | Primary Focus |
| :--- | :--- | :--- |
| **Classic Era / SoD** | `origin/classic_era` (Interface `11509`) | Deprecation fallbacks, `GetGuildInfo` restrictions on patch 1.15.x, difficulty colors |
| **TBC Classic Anniversary** | `origin/classic_anniversary` (Interface `20506`) | `C_SpecializationInfo` query structs, `GetActiveSpecGroup`, backdrop mixins |
| **Titanforge Wrath** | `origin/classic_era` (Interface `38001`) | Achievement points, Wrath talent trees, glyphs |

---

## 2. Specialization & Talent Query Architecture

Modern Classic clients (TBC Anniversary 2.5.6 and Era/SoD 1.15.9) have moved towards `C_SpecializationInfo`:

- **Query Struct:**

  ```lua
  C_SpecializationInfo.GetTalentInfo({
      specializationIndex = tab,
      talentIndex = index,
      isInspect = isInspect,
      isPet = false,
      groupIndex = group
  })
  ```

- **Fallback Pattern:**
  `LibClassicInspector` implements a wrapper around `GetTalentInfo` that transparently builds the `C_SpecializationInfo.GetTalentInfo` query struct when top-level `_G.GetTalentInfo` deprecation fallbacks are disabled.

- **Nil Safety:** Always guard `select(5, GetTalentInfo(...))` with `or 0` to prevent nil arithmetic errors on sparse talent builds.

---

## 3. Strict Retail Isolation

Retail-only APIs (e.g. `C_Traits`, modern WidgetManager anchors) must NEVER bleed into Classic execution paths. Live/Retail feature additions require strict `WOW_PROJECT_ID == WOW_PROJECT_MAINLINE` gating.
