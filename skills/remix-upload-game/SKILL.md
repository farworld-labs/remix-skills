---
name: remix-upload-game
description: Upload and validate HTML game code for Remix. Use when creating or updating a draft version through the CLI, MCP tools, or direct REST calls.
---

# Upload Game Workflow

## Overview

This skill guides you through uploading a completed HTML game to the Remix
platform. It covers creating the game entry (if needed) and uploading the code.

## Prerequisites

- A completed HTML game file, ready for upload.
- Prefer the official CLI for terminal workflows:
  - `remix login`
  - `remix games create --name "..."`
  - `remix games versions code update --code-path ./game.html`

## Steps

### 1. Locate the Game File

Find the path to the HTML file to upload. It should be a single self-contained
HTML document with inline CSS and JS.

### 2. Validate

Read the HTML file and verify:

1. Has `<!DOCTYPE html>` declaration
2. Has `<meta name="viewport" ...>` tag
3. Has RemixSDK script tag: `<script src="https://cdn.jsdelivr.net/npm/@remix-gg/sdk@latest/dist/index.min.js"></script>`
4. No legacy `@farcade/game-sdk` script tag
5. Calls `singlePlayer.actions.gameOver(` OR `multiplayer.actions.gameOver(` (not both)
6. Registers `.onPlayAgain(` callback
7. Registers `.onToggleMute(` callback
8. No `localStorage` or `sessionStorage` usage
9. No inline event handlers (`onclick=`, `onload=`, etc.)

Fix any reported issues before proceeding.

### 3. Create the Game (if needed)

First, check for a `.remix-settings.json` file in the project root. If it
exists and contains `gameId` and `versionId`, skip creation and proceed to
step 4.

If no settings file exists (or it lacks these IDs), create the game via CLI or REST.

CLI:

```bash
remix games create --name "My Game"
```

REST:

```
POST https://api.remix.gg/v1/games
Authorization: Bearer $REMIX_API_KEY
Content-Type: application/json

{ "name": "<game name>" }
```

The response returns the **game ID** and **version ID**.

After creation succeeds, write the returned IDs to `.remix-settings.json`:

```json
{
  "gameId": "<returned game ID>",
  "versionId": "<returned version ID>",
  "name": "<game name>"
}
```

### 4. Upload the Code

Read `gameId` and `versionId` from `.remix-settings.json` (or use the values
just obtained from creation).

Preferred CLI path:

```bash
remix games versions code update --code-path ./game.html
remix games versions validate get
remix games launch-readiness get
```

**Read the HTML file** and pass its content as the `code` field in the request body:

```
POST https://api.remix.gg/v1/games/{gameId}/versions/{versionId}/code
Authorization: Bearer $REMIX_API_KEY
Content-Type: application/json

{ "code": "<HTML file contents>" }
```

The response returns a confirmation with the game ID, version ID, and thread ID.

## Tips

- Always validate before uploading to catch issues early.
- You can re-upload code to the same version as many times as needed.
- `remix games versions status get` and `remix games versions thread get` are useful post-upload inspection commands.
- If you need a fresh draft, create a new game entry rather than inventing a version-create route.
