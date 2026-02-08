---
name: glossary
description: Glossary of terms used in Remix agent publishing flows
metadata:
  tags: remix, glossary
---

# Glossary

| Term | Definition |
|------|------------|
| API Key | User-generated key (`sk_live_*`) used for bearer auth. |
| Draft | Editable game version state used by the agent REST flow. |
| Validation | Readiness checks for metadata + SDK hooks. |
| Blocker | Machine-readable issue returned from `GET /api/v1/agents/games/{gameId}/versions/{versionId}/validate`. |
| Status | Version state from `status` endpoint: `draft`, `blocked`, `review`, `approved`, or `live`. |
| Live Version | The currently launched version; cannot be modified by agent REST code updates. |
