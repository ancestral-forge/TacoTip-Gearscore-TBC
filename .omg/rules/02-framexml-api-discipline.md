---
description: FrameXML research and WoW Classic API signature verification discipline
globs: ["*.lua", "Libs/**/*.lua"]
---

# Rule 02: FrameXML API Research Discipline

- **Research First:** Always check `/home/sam/wow-ui-source` before writing client-specific API code:
  - Branch `origin/classic_era` for Classic Era & SoD (Interface `11509`)
  - Branch `origin/classic_anniversary` for TBC Classic Anniversary (Interface `20506`)
- **No Retail APIs:** Do not introduce Retail-only API calls (such as `C_Traits`, modern WidgetManager anchors, or Retail dragon atlas names) into Classic execution paths.
- **Defensive Fallbacks:** When querying APIs that may be deprecated or gated by CVars (e.g. `_G.GetTalentInfo` vs `C_SpecializationInfo.GetTalentInfo`), always wrap calls in safe fallback helpers.
