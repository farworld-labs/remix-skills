---
name: api-authentication
description: Authentication for Remix agent publishing REST APIs using existing user API keys
metadata:
  tags: remix, authentication, api-key
---

## Current Auth Model

Remix uses existing user-generated API keys.

Supported auth header:
- `Authorization: Bearer sk_live_...` (recommended)

The backend resolves the key to a user and enforces ownership checks on game/version mutations.

## Required Headers

```http
Authorization: Bearer sk_live_xxx
Content-Type: application/json
```

## Security Notes

- Never expose API keys in client-side browser code.
- Store in server env vars or secret manager.
- Revoke and rotate keys from Remix Studio API Keys page.
- Key usage updates `lastUsed` automatically.
