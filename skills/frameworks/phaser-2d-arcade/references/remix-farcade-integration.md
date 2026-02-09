# Remix/Farcade Integration

Use this when Phaser output is intended for Remix/Farcade agent publishing.

## Required Hooks

Implement all of these:

- `window.FarcadeSDK.singlePlayer.actions.gameOver({ score })`
- `window.FarcadeSDK.onPlayAgain(() => resetGame())`
- `window.FarcadeSDK.onToggleMute(({ isMuted }) => { ... })`

## Initialization Pattern

1. Wait for `await window.FarcadeSDK.ready()`.
2. Register `onPlayAgain` and `onToggleMute` once.
3. Keep a `finishGame(score)` function that always calls `gameOver`.

## Submission Safety

- Keep final output as single-file HTML when using Remix agent code upload.
- If your game needs external images/audio, upload them in Remix Studio/app flow and reference URLs returned by `GET /v1/agents/games/{gameId}/assets`.
- Ensure score is numeric and stable at game end.
- Do not rely on localStorage for authoritative progression.
- Ensure restart path fully resets Phaser scene state.
