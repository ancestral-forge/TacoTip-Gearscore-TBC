# Locale Override Lifecycle & SavedVariables Invariant

## Root Cause & Mechanism
- TOC file execution occurs BEFORE SavedVariables are loaded into memory.
- In WoW, SavedVariables (like `TacoTipConfig`) are deserialized from `WTF/` and injected into `_G` immediately before the `ADDON_LOADED` event fires for the addon.
- Consequently, at TOC parse time when `Locale/*.lua` files execute:
  `TacoTipConfig` is ALWAYS `nil`.
  Therefore, expressions like `((TacoTipConfig and TacoTipConfig.locale_override) or GetLocale())` evaluate solely to `GetLocale()`.
- If a user configures `TacoTipConfig.locale_override = "zhCN"` on an `"enUS"` client, reloading the UI causes `Locale/zhCN.lua` to bail out because `TacoTipConfig` is nil and `GetLocale()` is `"enUS"`. Then `Locale/enUS.lua` loads and seals `_G.TACOTIP_LOCALE`.
- On `ADDON_LOADED`, `TacoTipConfig` is loaded, but no code re-populates or re-binds `_G.TACOTIP_LOCALE`.

## Enterprise Solution Pattern
1. Each `Locale/<loc>.lua` registers its translations into a table (e.g. `TACOTIP_LOCALES[locale] = { ... }`).
2. `Locale/enUS.lua` registers the default table.
3. On `ADDON_LOADED` (when `TacoTipConfig` is guaranteed available):
   Resolve `activeLocale = (TacoTipConfig and TacoTipConfig.locale_override) or GetLocale()`.
   Populate `_G.TACOTIP_LOCALE` in-place (mutating the existing table so module locals like `local L = _G.TACOTIP_LOCALE` retain the reference) by copying `defaults` and overlaying `TACOTIP_LOCALES[activeLocale]`.