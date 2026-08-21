---
description: Synchronous version bumping protocol across project manifests and documentation
globs: ["TacoTip.toc", "main.lua", "options.lua", "README.md", "CHANGELOG.md", "AGENTS.md"]
---

# Rule 06: Version Release Bumping Protocol

When bumping the addon version, update all 6 core files simultaneously:
1. `TacoTip.toc`: `## Version: X.Y.Z`
2. `main.lua`: `addOnVersion` fallback string (`"X.Y.Z"`)
3. `options.lua`: `addOnVersion` fallback string (`"X.Y.Z"`)
4. `README.md`: Public version badge/table and `## What's new in vX.Y.Z` section
5. `CHANGELOG.md`: Version summary table entry and detailed `## [X.Y.Z] - YYYY-MM-DD` section
6. `AGENTS.md`: Current version reference in Commands & Workflow
