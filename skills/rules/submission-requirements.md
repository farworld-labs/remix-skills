# Submission Requirements

A version should be considered submit-ready only when all are true:

- Game code exists
- Game has name, icon, and >=1 category
- SDK hooks exist in code:
  - `window.FarcadeSDK.singlePlayer.actions.gameOver(...)`
  - `window.FarcadeSDK.onPlayAgain(...)`
  - `window.FarcadeSDK.onToggleMute(...)`

## How to satisfy metadata requirements (current)

- Name: `game.updateName`
- Category: `game.addCategory` (1-3)
- Icon: Remix Studio UI/internal upload flow (no dedicated `agentPublish` icon mutation yet)

Use `agentPublish.validate` to confirm before submit.
