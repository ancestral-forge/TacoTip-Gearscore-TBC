# Ultragoal Brief: Dual-Spec Talent Styling Fix (Lowest GearScore Grey)

## Objective
Audit and fix dual-spec talent rendering so the active spec displays normal (class-colored spec name, white numbers) and the inactive spec displays greyed out (spec name in lowest GearScore quality grey, white numbers). Preserve `|c00000000` alignment prefix.

## Scope & Constraints
1. **Active vs Inactive Spec Styling:**
   - Active spec name receives full class color.
   - Inactive spec name receives lowest GearScore trash grey (`0.50, 0.50, 0.50` / `|cFF808080`).
   - Talent point allocation numbers (`[%d/%d/%d]`) remain white for both active and inactive specs.
2. **Alignment Preservation:**
   - The `|c00000000` prefix in compact tooltip mode (`main.lua`) is strictly preserved for icon alignment.
3. **Quality Gates & Memory Integrity:**
   - Zero-warning `luacheck .` compliance across all 21 files.
   - Update `Stats:DualSpecDimRendering` in `TacoTip_Tests.lua`.
   - Synchronize `MEMORY.md`, `.omg/memory/dual_spec_inspect.md`, `memory-bank/`, and `CHANGELOG.md`.
