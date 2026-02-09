---
name: remix-agent-publish
description: Build Remix games for remix.gg with the v1 agents REST API and Farcade game SDK requirements.
metadata:
  tags: remix, games, api, agent, publishing
---

## When to use

Use this skill when users want to automate game publishing on Remix (`remix.gg`) from AI agents or external services.

## How to use

- Read [API Authentication](api/authentication.md) first.
- Use [API Reference](api/reference.md) for endpoint contracts.
- Use [Game SDK Reference](references/game-sdk.md) when generating or fixing game code.
- Apply [Submission Rules](rules/submission-requirements.md) for validation requirements.
- Follow [Game Creation Best Practices](rules/game-creation-best-practices.md) for mobile-first and SDK-safe implementation.
- Use [REST Snippets](snippets/rest-client.md) for client integration examples.
- Use [MCP Quickstart](mcp/quickstart.md) for assistant workflows.

## Source of truth

When docs and runtime behavior disagree, defer to Farcade source:

- `apps/studio/app/api/v1/agents/games/route.ts`
- `apps/studio/app/api/v1/agents/games/[gameId]/versions/[versionId]/code/route.ts`
- `apps/studio/app/api/v1/agents/games/[gameId]/versions/[versionId]/validate/route.ts`
- `apps/studio/app/api/v1/agents/games/[gameId]/versions/[versionId]/status/route.ts`
- `apps/studio/lib/agent-api.ts`
- `packages/game-sdk/src/index.ts`
