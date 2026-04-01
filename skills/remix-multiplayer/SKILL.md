---
name: remix-multiplayer
description: Enable multiplayer support for a Remix game
---

# Implement Multiplayer Workflow

## Overview

This skill guides you through enabling and integrating multiplayer functionality
for games on the Remix platform.

## Prerequisites

- A game must already exist on the Remix platform.
- MCP or CLI auth should already be available through `REMIX_API_KEY` or saved CLI credentials from `remix login`.
- The game must include the RemixSDK script tag:
  ```html
  <script src="https://cdn.jsdelivr.net/npm/@remix-gg/sdk@latest/dist/index.min.js"></script>
  ```

## Steps

### 1. Check for Existing Game ID

Read task context first, then `.remix-cli.json`, then legacy `.remix-mcp.json`. If one of those sources contains a `gameId`, use that value.

Only if none of those sources contains a `gameId` should you
follow the **upload-game** workflow to create one.

### 2. Enable Multiplayer

The multiplayer flag is currently enabled by Remix staff after review, not by a public creator API or `updateGame` call. Do not invent an `isMultiplayer` update route in local docs or generated code.

### 3. Integrate Multiplayer in Game Code

```js
const sdk = window.RemixSDK
await sdk.ready()

const player = sdk.player
const players = sdk.players
const opponent = players.find((entry) => entry.id !== player.id)

// Listen for game state updates from other players
sdk.onGameStateUpdated((data) => {
  if (!data) return
  // Sync gameState from the other player
})

// Send updated turn state to the opponent
sdk.multiplayer.actions.saveGameState({
  gameState: { /* board state */ },
  alertUserIds: [opponent.id],
})
```

### Important: Use Multiplayer gameOver

For multiplayer games, use `multiplayer.actions.gameOver` -- NOT
`singlePlayer.actions.gameOver`. Pass a `scores` array:

```js
sdk.multiplayer.actions.gameOver({
  scores: [
    { playerId: player.id, score: myScore },
    { playerId: opponent.id, score: opponentScore },
  ]
})
```

### Keep Standard Hooks

Multiplayer games still need these callbacks:

```js
sdk.onPlayAgain(() => {
  // Reset game state and restart
})

sdk.onToggleMute(({ isMuted }) => {
  // Mute or unmute all audio
})
```

## Tips

- Use `sdk.players` to get the list of players and their IDs.
- `players[0]` is the opening player in a new match.
- `onGameStateUpdated` fires whenever another player sends state -- use it to
  keep all clients in sync.
- Use `multiplayer.actions.refuteGameState({ gameStateId })` if an incoming move is invalid.
- Do NOT mix `singlePlayer.actions.gameOver` and `multiplayer.actions.gameOver`
  in the same game. Use one or the other.
