---
name: trpc-client-snippets
description: tRPC snippets for Remix agent publishing
metadata:
  tags: remix, trpc, snippets
---

## TypeScript (tRPC client)

```ts
const draft = await trpc.agentPublish.createDraft.mutate({ name: 'Neon Dash' })

await trpc.agentPublish.uploadCode.mutate({
  gameId: draft.gameId,
  versionId: draft.versionId!,
  code: html,
})

const report = await trpc.agentPublish.validate.query({
  gameId: draft.gameId,
  versionId: draft.versionId!,
})

if (!report.valid) {
  throw new Error(`Blocked: ${report.blockers.map((b) => b.code).join(', ')}`)
}

await trpc.agentPublish.submit.mutate(
  {
    gameId: draft.gameId,
    versionId: draft.versionId!,
    releaseNotes: 'Initial submission',
    releaseTypes: { isBugFix: false, isNewFeature: true, isNewGame: true },
  },
  {
    context: {
      headers: {
        'Idempotency-Key': `submit-${draft.versionId}`,
      },
    },
  },
)
```
