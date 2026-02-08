---
name: api-authentication
description: Authentication for Remix agent publishing APIs using existing user API keys
metadata:
  tags: remix, authentication, api-key
---

## Current Auth Model

For this pass, Remix uses existing user-generated API keys.

Supported auth headers:
- `Authorization: Bearer sk_live_...` (recommended)
- `x-api-key: sk_live_...` (supported alternative)

Use one of the above. Do not send both unless your client stack requires it.

The backend resolves the key to a user and enforces ownership checks on game/version mutations.

## Required Headers

### All authenticated calls

Use either form:

```http
Authorization: Bearer sk_live_xxx
Content-Type: application/json
```

or

```http
x-api-key: sk_live_xxx
Content-Type: application/json
```

### Submit calls (required idempotency)

```http
Idempotency-Key: <stable-unique-key>
```

## Security Notes

- Never expose API keys in client-side browser code.
- Store in server env vars or secret manager.
- Revoke and rotate keys from Remix Studio API Keys page.
- Key usage updates `lastUsed` automatically.
