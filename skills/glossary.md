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
| Draft | Game version state before submit/review. |
| Validation | Pre-submit checks for metadata + SDK hooks. |
| Blocker | Machine-readable issue returned from `agentPublish.validate`. |
| Review | Status after successful submit and before approval/launch. |
| Idempotency Key | Request key required on submit to prevent duplicate submissions. |
