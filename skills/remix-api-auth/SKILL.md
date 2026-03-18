---
name: remix-api-auth
description: Configure and verify authentication for Remix REST, CLI, and MCP workflows. Use when a task needs `REMIX_API_KEY`, `remix login`, stored Remix credentials, or auth troubleshooting.
metadata:
  tags: remix, auth, api-key, authentication
---

# Remix Authentication Setup

Use this skill when a user needs to authenticate a terminal, MCP client, or external service against Remix.

## Auth Modes

- REST or custom services: send `Authorization: Bearer <api_key>` to `https://api.remix.gg`.
- CLI: run `remix login`, then verify with `remix whoami`.
- MCP: prefer `REMIX_API_KEY`; if unset, the server can reuse CLI credentials from `~/.config/remix/credentials.json`.

## Preferred Terminal Flow

```bash
curl -fsSL https://remix.gg/install.sh | bash
remix login
remix whoami
```

The CLI stores credentials in `~/.config/remix/credentials.json`.

## API Key Flow

1. Log in to your Remix account.
2. Go to `https://remix.gg/api-keys`.
3. Create a new API key.
4. Store it as a server-side secret or export `REMIX_API_KEY`.
5. Use base URL `https://api.remix.gg`.

## Verification

Use one of these checks first:

- `remix whoami`
- `remix config where`
- `GET /health`
- `GET /v1/games` if the user explicitly wants an authenticated data call

## Troubleshooting Invalid API Key

- Check whether `REMIX_API_KEY` is overriding stored CLI credentials.
- Check `Authorization` is formatted as `Bearer <api_key>`.
- Re-copy the key from `https://remix.gg/api-keys` and rotate if needed.
- Verify your service is reading the expected secret/env var in the current runtime.
- Confirm the request is server-side and not exposed through browser code.
- Run `remix whoami --json` to inspect the resolved auth/config context.
- If behavior seems inconsistent with local docs, use `https://api.remix.gg/docs` as source of truth.

## Current Auth Model

Server API base URL: `https://api.remix.gg`

Remix server API uses bearer API keys generated from Remix account settings.

Supported auth header:

- `Authorization: Bearer <api_key>` for direct HTTP usage

Runtime precedence in the CLI is:

1. explicit command flags
2. environment variables
3. stored config and credentials
4. defaults

The backend resolves the key to a user and enforces ownership checks on game/version mutations.

## How to Get an API Key

1. Log in to your Remix account.
2. Go to `https://remix.gg/api-keys`.
3. Generate a new API key.
4. Copy it once and store it in your server-side secret manager.

## Required Headers

```http
Authorization: Bearer <api_key>
Content-Type: application/json
```

## Security Notes

- Never expose API keys in client-side browser code.
- Store in server env vars or secret manager.
- Revoke and rotate keys from `https://remix.gg/api-keys`.
- Usage updates `lastUsed` automatically.

## API Authentication Rules

- Use API keys created from `https://remix.gg/api-keys`.
- Use `Authorization: Bearer <api_key>`.
- Send requests to `https://api.remix.gg`.
- Do not place keys in browser-side code.
- Rotate keys immediately if leaked.
- Account must not be banned or deleted.
