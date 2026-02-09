---
name: mcp-quickstart
description: Quickstart workflow for AI assistants using Remix publishing APIs
metadata:
  tags: remix, mcp, quickstart
---

## Recommended Flow

Base URL: `https://api.remix.gg`

1. `GET /v1/agents/metadata/categories` to fetch valid category enums.
2. `POST /v1/agents/games`
3. Build game code against `window.FarcadeSDK` requirements (see `references/game-sdk.md`)
4. Set required metadata:
   - Name: game metadata APIs
   - Category: game metadata APIs (1-3)
   - Icon: Remix Studio/internal upload flow currently
5. `PUT /v1/agents/games/{gameId}/versions/{versionId}/code`
6. `GET /v1/agents/games/{gameId}/versions/{versionId}/validate`
7. Optional: `GET /v1/agents/games/{gameId}/launch-readiness?versionId={versionId}`
8. If blockers exist, patch code/metadata and repeat validation
9. `GET /v1/agents/games/{gameId}/versions/{versionId}/status`

## Discovery / Inspection Endpoints

- `GET /v1/agents/games`
- `GET /v1/agents/games/{gameId}`
- `GET /v1/agents/games/{gameId}/versions`
- `GET /v1/agents/games/{gameId}/versions/{versionId}`
- `GET /v1/agents/games/{gameId}/versions/{versionId}/code`
- `GET /v1/agents/games/{gameId}/versions/{versionId}/thread`
- `GET /v1/agents/games/{gameId}/assets`

## Guardrails

- Never skip validation checks.
- Treat `blockers[]` as source of truth.
- Do not create extra versions from agent REST.
- Do not submit from agent REST.
- If docs are stale, check `https://api.remix.gg/openapi`.
