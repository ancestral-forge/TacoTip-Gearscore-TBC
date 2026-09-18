# Conventions

- Preserve shared namespaces TT, TT_GS, TT_PAWN; avoid new globals and retail-only APIs.
- Match neighboring Lua style; retain existing comments. Lua 5.1 compatibility is required.
- Static tooltip label prefixes require explicit inline white color codes; friendly player levels are white, difficulty colors reserved for hostile/attackable units.
- clearTooltipVisuals must be nil-safe and immediately hide all unit overlays/reset borders on show/clear transitions; item/spell/map tooltips must not inherit unit visuals.
- High-frequency tooltip formatting uses pooledLinesToAdd and pooledTooltipText; avoid per-hover allocations.
- SharedMedia resolution uses cached TT:GetResolvedTooltip* lookups with TT:InvalidateResolvedMediaCache(), not per-hover choice rebuilding/sorting.
- Power-bar and UNIT_TARGET handlers return immediately when the relevant bar/tooltip is hidden.
- Options reuse existing TacoTipConfig keys. RefreshOptionsUI synchronizes controls after mover/overlay changes; SyncTooltipMover reanchors the mover after anchor/reset changes.
- Add localization strings to Locale/enUS.lua first; other locales use the existing English fallback merge until translated.
- Research Blizzard API signatures against the appropriate Classic reference branch before API-specific edits.
