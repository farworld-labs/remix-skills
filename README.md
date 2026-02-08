# Remix Skills

Best practices, references, and templates for building games on Remix via API-driven agent workflows.

## Installation

```bash
npx skills add farworld-labs/remix-skills
```

## Skill Contents

| Topic | Description |
|-------|-------------|
| [API Reference](skills/api/reference.md) | Endpoint reference for `/api/v1/agents/*` |
| [API Authentication](skills/api/authentication.md) | Using existing Remix API keys |
| [Auth Setup](skills/auth/SKILL.md) | How to create/use API keys for agents |
| [Submission Rules](skills/rules/submission-requirements.md) | Validation and publish constraints |
| [MCP Quickstart](skills/mcp/quickstart.md) | Suggested tool workflow for assistants |
| [REST Snippets](skills/snippets/rest-client.md) | Copy-paste client examples |

## Quick Start

```ts
// 1) create draft game
const create = await fetch(`${baseUrl}/api/v1/agents/games`, {
  method: 'POST',
  headers,
  body: JSON.stringify({ name: 'Neon Dash' }),
}).then((r) => r.json())

const gameId = create.data.game.id
const versionId = create.data.game.version.id

// 2) update current version code
await fetch(`${baseUrl}/api/v1/agents/games/${gameId}/versions/${versionId}/code`, {
  method: 'PUT',
  headers,
  body: JSON.stringify({ code: html }),
})

// 3) validate blockers
const report = await fetch(
  `${baseUrl}/api/v1/agents/games/${gameId}/versions/${versionId}/validate`,
  { method: 'GET', headers },
).then((r) => r.json())

// 4) check status
await fetch(`${baseUrl}/api/v1/agents/games/${gameId}/versions/${versionId}/status`, {
  method: 'GET',
  headers,
})
```

## Auth Model (Current Pass)

This repo documents the current model:
- existing Remix user-created API keys
- bearer auth (`Authorization: Bearer sk_live_...`)
- ownership checks + lightweight rate limiting
- no agent REST submit endpoint
