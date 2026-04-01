---
name: remix-game-creation
description: Create a new game draft via the Remix API
---

# Game Creation Workflow

## Overview

This skill guides you through creating a new HTML game on the Remix platform.

## Prerequisites

- Auth should already be available through `REMIX_API_KEY` or saved CLI credentials from `remix login`.

## Constraints

- The game should support both `feed` and `full_screen` mobile contexts.
- In `full_screen`, keep critical HUD and controls inside `gameInfo.contentSafeAreaInset`.
- Decorative art can bleed edge-to-edge, but important UI should not.
- The game **must support touch controls** as the primary input method (the Remix
  platform is primarily mobile/touch-based). Keyboard and mouse controls are
  welcome but optional.
- Prefer starting gameplay immediately instead of adding a separate start screen.

## Steps

### 1. Plan the Game

Decide on the game concept, mechanics, and visual style. Keep scope small --
a single-file HTML game should be playable in under 30 seconds.

Plan layout around `await window.RemixSDK.ready()` and `window.RemixSDK.gameInfo`
instead of assuming one fixed aspect ratio.

### 2. Write the HTML

Create a single HTML file with inline `<style>` and `<script>` tags. Structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Game Title</title>
  <script src="https://cdn.jsdelivr.net/npm/@remix-gg/sdk@latest/dist/index.min.js"></script>
  <style>
    /* Game styles can go here */
  </style>
</head>
<body>
  <!-- Game markup can go here -->
  <script>
    // Game logic here
  </script>
</body>
</html>
```

### Required RemixSDK Functions

1. **`window.RemixSDK.singlePlayer.actions.gameOver({ score })`** -- Call when the game
   ends, passing the player's final score as `{ score: number }`. This reports
   the score to the platform.

2. **`window.RemixSDK.onPlayAgain(callback)`** -- Register a callback (no arguments)
   that fires when the player requests to play again. The callback should reset
   game state and restart the game.

3. **`window.RemixSDK.onToggleMute(callback)`** -- Register a callback that receives
   `{ isMuted: boolean }`. The game must mute or unmute all audio based on the
   `isMuted` value.

### 3. Validate

Before uploading, read the HTML file and verify:

1. Has `<!DOCTYPE html>` declaration
2. Has `<meta name="viewport" ...>` tag
3. Has RemixSDK script tag: `<script src="https://cdn.jsdelivr.net/npm/@remix-gg/sdk@latest/dist/index.min.js"></script>`
4. No legacy `@farcade/game-sdk` script tag
5. Calls `singlePlayer.actions.gameOver(` OR `multiplayer.actions.gameOver(` (not both)
6. Registers `.onPlayAgain(` callback
7. Registers `.onToggleMute(` callback
8. No `localStorage` or `sessionStorage` usage
9. No inline event handlers (`onclick=`, `onload=`, etc.)

Fix any issues before proceeding.

### 4. Upload the Game

Follow the **upload-game** workflow to create the game on the Remix platform and
deploy your HTML code.

## Tips

- Use `requestAnimationFrame` for game loops, not `setInterval`.
- Touch controls are required (primary input); keyboard/mouse are optional extras.
- Use `window.RemixSDK.hapticFeedback()` for important mobile moments instead of `navigator.vibrate()`.
- Treat `viewContext === "full_screen"` as a safe-area-aware layout mode, not a letterboxed 2:3 viewport.
- Keep the total HTML under 100KB for fast loading.
