---
name: remix-open-game
description: Open a game in the Remix Studio browser for preview and editing
---

# Open Game in Remix Studio

Open the current game in the Remix Studio for preview and editing.

## Steps

1. **Read game settings** from task context, prior tool results, the nearest
   `.remix-cli.json`, or legacy `.remix-mcp.json`. Extract `gameId` and
   `versionId`. If either is missing, tell the user they need to create or
   upload a game first.

2. **Construct the Studio URL:**
   ```
   https://remix.gg/games/{gameId}/v/{versionId}
   ```

3. **Open in the user's default browser:**
   - macOS: `open <url>`
   - Linux: `xdg-open <url>`
