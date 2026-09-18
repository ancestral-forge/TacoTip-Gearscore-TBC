# Zero-Allocation Hotpaths & Garbage Collector Invariants

## Identified Allocation Leaks in Tooltip Pipeline
1. **`linesToAdd` Subtable Creation (`main.lua:928-1310`):**
   Although `linesToAdd` uses `pooledLinesToAdd`, 10–15 anonymous tables `{ L["Target"] .. ":", ... }` are instantiated and pushed via `tinsert(linesToAdd, { ... })` on every hover.
   Fix: Maintain a pool of reusable sub-tables (e.g. `pooledLineRecords[i]`), or use flat indexing `linesToAdd[i*8 + offset]`.
2. **`newText` Instantiation (`main.lua:1077`):**
   `local newText = {}` allocates a new table on every player hover, defeating `pooledTooltipText`.
   Fix: Use a secondary static pool `pooledNewText = {}` or mutate `pooledTooltipText` in-place.
3. **`itemInfo` Instantiation (`main.lua:1513`):**
   `local itemInfo = { GetItemInfo(itemLink) }` allocates a table of ~17 elements on every item hover.
   Fix: Use a static `pooledItemInfo` buffer or extract itemLevel, quality, and equipLoc via direct multiple return values.

## Performance Impact
In crowded raid encounters, battlegrounds, or cities with high-frequency mouseovers, eliminating these allocations reduces Lua GC cycle spikes, preventing frame drops during player mouse tracking.