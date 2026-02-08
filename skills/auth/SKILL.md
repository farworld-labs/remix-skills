---
name: remix-api-key-auth
description: Configure and verify API-key authentication for Remix agent publishing workflows.
metadata:
  tags: remix, auth, api-key
---

# Remix API Key Setup

Use this skill when a user needs to authenticate an external service/agent for Remix publishing APIs.

## Steps

1. In Remix Studio, create a new API key.
2. Store it as a secret in your service runtime.
3. Send:
   - `Authorization: Bearer <api_key>`

## Verification

Run a cheap authenticated call first (for example, `POST /api/v1/agents/games` in a test project) to verify the key works.
