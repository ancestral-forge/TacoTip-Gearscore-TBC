---
description: Zero-warning Luacheck static analysis gate across all repository files
alwaysApply: true
---

# Rule 01: Zero-Warning Luacheck Standard

- **Strict Zero Tolerance:** All modified and new Lua code must pass `luacheck .` with 0 warnings and 0 errors.
- **No Suppression Comments:** Never add `-- luacheck: ignore` comments to silence warnings.
- **Safe Dynamic Table Access:** If mutating or accessing global frame fields (like `GameTooltip["Method"]` or `_G["WorldMapTooltip"]`), use dynamic indexing rather than raw global variable declarations.
- **Local Scope Management:** Always localize imported functions and loop variables. Never leak variables to the global scope.
