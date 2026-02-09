---
name: mcp-quickstart
description: Quickstart workflow for AI assistants using Remix publishing APIs
metadata:
  tags: remix, mcp, quickstart
---

## Recommended Flow

1. `POST /api/v1/agents/games`
2. Build game code against `window.FarcadeSDK` requirements (see `references/game-sdk.md`)
3. Set required metadata:
   - Name: game metadata APIs
   - Category: game metadata APIs (1-3)
   - Icon: Remix Studio/internal upload flow currently
4. `PUT /api/v1/agents/games/{gameId}/versions/{versionId}/code`
5. `GET /api/v1/agents/games/{gameId}/versions/{versionId}/validate`
6. If blockers exist, patch code/metadata and repeat validation
7. `GET /api/v1/agents/games/{gameId}/versions/{versionId}/status`

## Guardrails

- Never skip validation checks.
- Treat `blockers[]` as source of truth.
- Do not create extra versions from agent REST.
- Do not submit from agent REST.
