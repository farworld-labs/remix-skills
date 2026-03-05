---
name: remix-agent-publish
description: Build Remix games for remix.gg with the server-api v1 REST API and Remix Game SDK requirements.
metadata:
  tags: remix, games, api, agent, publishing
---

## When to use

Use this skill when users want to automate game publishing on Remix (`remix.gg`) from AI agents or external services.

## How to use

- Read [API Authentication](api/authentication.md) first.
- Fetch OpenAPI spec at `https://api.remix.gg/docs/json` before generating API calls.
- Use [API Reference](api/reference.md) for workflow guardrails (not as the method/path source of truth).
- For Phaser builds, use [Phaser 2D Arcade Companion Skill](frameworks/phaser-2d-arcade/SKILL.md).
- For lightweight 3D builds, use [Three.js Lite Companion Skill](frameworks/threejs-lite/SKILL.md).
- Use [Game SDK Reference](references/game-sdk.md) when generating or fixing game code.
- Apply [Submission Rules](rules/submission-requirements.md) for validation requirements.
- Follow [Game Creation Best Practices](rules/game-creation-best-practices.md) for mobile-first and SDK-safe implementation.
- Use [REST Snippets](snippets/rest-client.md) for client integration examples.
- Use [MCP Quickstart](mcp/quickstart.md) for assistant workflows.

## Available Workflows

- [Game Creation](workflows/game-creation.md) - Create a new game draft
- [Upload Game](workflows/upload-game.md) - Upload version code
- [Add Image](workflows/add-image.md) - Generate and add images to a game
- [Add Sprite](workflows/add-sprite-to-game.md) - Generate and add sprites to a game
- [Manage Shop Items](workflows/manage-shop-items.md) - Create and manage in-game shop items
- [Implement Multiplayer](workflows/implement-multiplayer.md) - Enable multiplayer support
- [Integrate Save Game](workflows/integrate-save-game.md) - Add save/load game state
- [Upload Game Asset](workflows/upload-game-asset.md) - Upload images, audio, 3D models, or video as hosted game assets

## Game Settings

Games can include a `.remix-settings.json` file for configuration. See workflow files for details on when and how to use game settings.

## Inline Validation

Before uploading code, run through the validation checklist in [Submission Rules](rules/submission-requirements.md) to catch blockers early.

## REST API Workflow

All game management is done through REST API calls to `https://api.remix.gg`. The general pattern is:

1. Fetch the OpenAPI spec at `https://api.remix.gg/docs/json`
2. Use the spec to determine exact methods, paths, and schemas
3. Make authenticated REST calls with your API key
4. Check responses for success/error envelopes

See [REST Snippets](snippets/rest-client.md) and [API Reference](api/reference.md) for details.

## Source of truth

When docs and runtime behavior disagree, defer to the server API source and OpenAPI docs:

- `https://api.remix.gg/docs`
- `https://api.remix.gg/docs/json`
- `apps/server-api/routes/agents.ts`
- `apps/server-api/lib/api-auth.ts`
- `apps/server-api/lib/agent-api.ts`
- `apps/server-api/app/[...path]/route.ts`
- `@remix-gg/sdk` (NPM package)
