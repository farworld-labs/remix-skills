---
name: mcp-quickstart
description: Quickstart workflow for AI assistants using Remix publishing APIs
metadata:
  tags: remix, mcp, quickstart
---

## Recommended Flow

1. `agentPublish.createDraft`
2. Set required metadata before submit:
   - Name: `game.updateName`
   - Category: `game.addCategory` (1-3)
   - Icon: set via Remix Studio currently (no `agentPublish` icon mutation yet)
3. `agentPublish.uploadCode`
4. `agentPublish.validate`
5. If blockers exist, patch code/metadata and repeat validation
6. `agentPublish.submit` (with idempotency key)
7. Poll `agentPublish.status`

## Guardrails

- Never submit without a validation pass.
- Treat `blockers[]` as source of truth.
- Reuse idempotency keys on retries for the same submit intent.
