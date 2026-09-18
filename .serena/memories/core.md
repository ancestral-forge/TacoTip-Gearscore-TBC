# Core Map

- WoW Classic-family tooltip/GearScore/iLvl addon; retail unsupported. Supported interface IDs are declared in TacoTip.toc (11509, 20506, 38001); do not infer support solely from broad runtime build guards.
- Runtime load order: bundled libraries -> locales (enUS last) -> gearscore.lua -> pawn.lua -> textures.lua -> options.lua -> main.lua -> TacoTip_Tests.lua.
- Shared state: TT core namespace, TT_GS scoring, TT_PAWN integration, TacoTipConfig SavedVariables, TACOTIP_LOCALE localization.
- gearscore.lua: scoring and bootstrap; pawn.lua: Pawn bridge; textures.lua: media choices; options.lua: defaults/config sanitization/settings pages; main.lua: live tooltips, event lifecycle, mover, character/inspect overlays; TacoTip_Tests.lua: WoWUnit tests.
- Options support modern Settings canvas registration and legacy InterfaceOptions fallback. Root/general plus Tooltips, Positioning, Character & Inspect; no Advanced page.
- Language override uses TacoTipConfig.locale_override on reload; enUS is the fallback/source of truth.

## Navigation
- Runtime/tooling constraints: `mem:tech_stack`.
- Tooltip performance, isolation, and localization invariants: `mem:conventions`.
- Lint, in-game testing, and non-mutating API-reference commands: `mem:suggested_commands`.
- Verification and synchronized documentation gates: `mem:task_completion`.
- Specialized audit notes, to revalidate against current source before acting: `mem:audit/locale_override_lifecycle`, `mem:audit/nineslice_background_rendering`, `mem:audit/tooltip_geometry_and_padding`, `mem:audit/zero_allocation_hotpaths`.
