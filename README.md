# Remix Skills

Best practices, references, and templates for building games on Remix via API-driven agent workflows.

## What is Remix.gg?

[Remix.gg](https://remix.gg) is a platform for creating, publishing, and iterating on web games. These docs focus on using Remix's agent APIs so assistants and backend services can programmatically create and manage game drafts, code updates, and validation checks.

## Installation

```bash
npx skills add farworld-labs/remix-skills
```

## Skill Contents

| Topic | Description |
|-------|-------------|
| [API Reference](skills/api/reference.md) | Endpoint reference for `https://api.remix.gg/v1/agents/*` |
| [API Authentication](skills/api/authentication.md) | Using bearer API keys from Remix account settings |
| [Auth Setup](skills/auth/SKILL.md) | How to create/use API keys for agents |
| [Submission Rules](skills/rules/submission-requirements.md) | Validation and publish constraints |
| [MCP Quickstart](skills/mcp/quickstart.md) | Suggested tool workflow for assistants |
| [REST Snippets](skills/snippets/rest-client.md) | Copy-paste client examples |
| [Phaser 2D Arcade Skill](skills/frameworks/phaser-2d-arcade/SKILL.md) | Companion build skill for Phaser browser games |
| [Three.js Lite Skill](skills/frameworks/threejs-lite/SKILL.md) | Companion build skill for lightweight 3D browser games |

## API Key Setup

1. Log in to your Remix account.
2. Open `https://remix.gg/api-keys`.
3. Generate a new API key.
4. Store it securely and use it as a bearer token.

## Quick Start

```ts
const baseUrl = 'https://api.remix.gg'

// 0) fetch the live contract first
const openApiSpec = await fetch(`${baseUrl}/docs/json`).then((r) => r.json())

// 1) create draft game
const create = await fetch(`${baseUrl}/v1/agents/games`, {
  method: 'POST',
  headers,
  body: JSON.stringify({ name: 'Neon Dash' }),
}).then((r) => r.json())

const gameId = create.data.game.id
const versionId = create.data.game.version.id

// 2) update current version code
await fetch(`${baseUrl}/v1/agents/games/${gameId}/versions/${versionId}/code`, {
  method: 'POST',
  headers,
  body: JSON.stringify({ code: html }),
})

// 3) validate blockers
const report = await fetch(
  `${baseUrl}/v1/agents/games/${gameId}/versions/${versionId}/validate`,
  { method: 'GET', headers },
).then((r) => r.json())

// 4) check status
await fetch(`${baseUrl}/v1/agents/games/${gameId}/versions/${versionId}/status`, {
  method: 'GET',
  headers,
})
```

## Additional Read Endpoints

- `GET /v1/agents/metadata/categories`
- `GET /v1/agents/games`
- `GET /v1/agents/games/{gameId}`
- `GET /v1/agents/games/{gameId}/versions`
- `GET /v1/agents/games/{gameId}/versions/{versionId}`
- `GET /v1/agents/games/{gameId}/versions/{versionId}/code`
- `GET /v1/agents/games/{gameId}/versions/{versionId}/thread`
- `GET /v1/agents/games/{gameId}/assets`
- `GET /v1/agents/games/{gameId}/launch-readiness?versionId={versionId}`

## Asset Uploads (Current)

- As of February 9, 2026, asset upload is handled through the Remix app/Studio flow.
- Agent REST currently exposes `GET /v1/agents/games/{gameId}/assets` for read-only asset discovery.
- Recommended workflow: upload icon/sprites/audio in the app first, then reference returned hosted URLs in game code.

## Auth Model (Current Pass)

This repo documents the current model:
- API keys created in `https://remix.gg/api-keys`
- bearer auth (`Authorization: Bearer <api_key>`)
- ownership checks + lightweight rate limiting
- no agent REST submit endpoint
- docs fallback at `https://api.remix.gg/docs`
