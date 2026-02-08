# Remix Agent Publish Skills

Best practices, references, and templates for building and launching games on Remix via API-driven agent workflows.

## Installation

```bash
npx skills add remix-gg/skills
```

## Skill Contents

| Topic | Description |
|-------|-------------|
| [API Reference](skills/api/reference.md) | Endpoint reference for `agentPublish.*` |
| [API Authentication](skills/api/authentication.md) | Using existing Remix API keys |
| [Auth Setup](skills/auth/SKILL.md) | How to create/use API keys for agents |
| [Submission Rules](skills/rules/submission-requirements.md) | Required checks before submit |
| [MCP Quickstart](skills/mcp/quickstart.md) | Suggested tool workflow for assistants |
| [tRPC Snippets](skills/snippets/trpc-client.md) | Copy-paste client examples |

## Quick Start

```ts
// 1) create draft
await trpc.agentPublish.createDraft.mutate({ name: 'Neon Dash' })

// 2) upload code
await trpc.agentPublish.uploadCode.mutate({ gameId, versionId, code: html })

// 3) validate
const report = await trpc.agentPublish.validate.query({ gameId, versionId })

// 4) submit (requires Idempotency-Key header)
await trpc.agentPublish.submit.mutate({ gameId, versionId })
```

## Auth Model (Current Pass)

This repo documents the current simplified model:
- existing Remix user-created API keys
- bearer auth (`Authorization: Bearer sk_live_...`)
- ownership checks + lightweight rate limiting + idempotency on submit
