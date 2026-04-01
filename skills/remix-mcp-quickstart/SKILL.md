---
name: remix-mcp-quickstart
description: Quickstart for the official Remix MCP server. Use when configuring or operating `@remix-gg/mcp`, wiring MCP auth, or choosing the right Remix MCP tools and skill resources.
metadata:
  tags: remix, mcp, quickstart
---

## Install And Auth

Install the human-facing CLI if you want browser auth and reusable credentials:

```bash
curl -fsSL https://remix.gg/install.sh | bash
remix login
remix whoami
```

Then configure MCP:

```bash
claude mcp add remix-mcp -- npx -y @remix-gg/mcp
```

The MCP server reads auth from:

1. `REMIX_API_KEY`
2. saved CLI credentials in `~/.config/remix/credentials.json`

## Project Settings

The MCP server does not persist IDs for you. Reuse IDs from:

1. task context or prior tool results
2. nearest `.remix-cli.json`
3. legacy `.remix-mcp.json`

Before calling `createGame`, check those sources first. If IDs already exist, reuse them instead of creating a duplicate game. If the workflow needs IDs to persist across sessions, write `gameId` and `versionId` into `.remix-cli.json`.

## Core Tool Set

- `createGame`
- `updateGame`
- `uploadVersion`
- `generateImage`
- `generateShopImage`
- `uploadGameAsset`
- `generateSpriteSheet`
- `listShopItems`
- `createShopItem`
- `updateShopItem`
- `deleteShopItem`

## Skill Resources

The server exposes skill resources under `skills://...`:

- `skills://overview`
- `skills://game-creation`
- `skills://upload-game`
- `skills://implement-multiplayer`
- `skills://integrate-save-game`
- `skills://add-image-to-game`
- `skills://manage-shop-items`
- `skills://add-sprite-to-game`
- `skills://migrate-mobile-fullscreen`
- `skills://open-game`

Load the matching skill resource before taking multi-step actions.

## Recommended Flow

1. Read task context first, then `.remix-cli.json`, then legacy `.remix-mcp.json`.
2. If no IDs exist, call `createGame`.
3. Build or repair the game against `window.RemixSDK` requirements.
4. Upload assets with `uploadGameAsset`, `generateImage`, `generateShopImage`, or `generateSpriteSheet` as needed.
5. Upload code with `uploadVersion`.
6. If the task involves monetization, run `listShopItems` before creating or updating items.
7. Use the matching workflow resource for detailed rules before continuing.

## Guardrails

- Do not call `createGame` when task context, `.remix-cli.json`, or legacy `.remix-mcp.json` already has IDs.
- Do not read the HTML or asset file yourself before calling file-path-based MCP tools.
- Use CLI login or `REMIX_API_KEY`; do not invent a separate MCP-only auth scheme.
- Use `REMIX_API_BASE_URL` only when overriding the MCP server API base URL.
- If package docs disagree, prefer the current package source in `packages/mcp/src`.
