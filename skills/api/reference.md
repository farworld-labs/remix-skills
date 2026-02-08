---
name: api-reference
description: API reference for Remix agent publishing REST routes
metadata:
  tags: remix, api, rest, reference
---

## Transport

Current agent publishing endpoints are exposed as REST routes under `/api/v1/agents`.

Base headers:

```http
Authorization: Bearer sk_live_xxx
Content-Type: application/json
```

## Response Envelope

Success:

```json
{ "success": true, "data": { "...": "..." } }
```

Error:

```json
{ "success": false, "error": { "code": "SOME_CODE", "message": "...", "details": null } }
```

## Routes

### `POST /api/v1/agents/games`
Create a new game draft and initial version.

Input:

```json
{ "name": "Neon Dash" }
```

Output (`201`):

```json
{
  "success": true,
  "data": {
    "game": {
      "id": "...",
      "name": "Neon Dash",
      "createdAt": "...",
      "version": {
        "id": "...",
        "createdAt": "..."
      }
    }
  }
}
```

Constraints:
- User must be authenticated and not banned/deleted.
- Enforces in-development game cap (`20`) unless admin.

### `PUT /api/v1/agents/games/{gameId}/versions/{versionId}/code`
Update code for an existing version.

Input:

```json
{ "code": "<html>...</html>" }
```

Output (`200`):

```json
{
  "success": true,
  "data": {
    "success": true,
    "gameId": "...",
    "versionId": "...",
    "threadId": "..."
  }
}
```

Constraints:
- Scoped to owner (or admin).
- Code size must be between `10` and `1_000_000` characters.
- Updates existing version only.
- Cannot update live version.
- Cannot update submitted/launched/approved versions.

### `GET /api/v1/agents/games/{gameId}/versions/{versionId}/validate`
Returns machine-readable blockers for readiness checks.

Output (`200`):

```json
{
  "success": true,
  "data": {
    "gameId": "...",
    "versionId": "...",
    "valid": false,
    "blockers": [
      {
        "code": "MISSING_SDK_TOGGLE_MUTE",
        "message": "SDK onToggleMute handler not found.",
        "fix": "Add window.FarcadeSDK.onToggleMute((data) => { ... })."
      }
    ]
  }
}
```

### `GET /api/v1/agents/games/{gameId}/versions/{versionId}/status`
Get publishing status for a version.

Output (`200`):

```json
{
  "success": true,
  "data": {
    "gameId": "...",
    "versionId": "...",
    "status": "draft|blocked|review|approved|live",
    "submittedAt": null,
    "approvedAt": null,
    "launchedAt": null,
    "feedback": null
  }
}
```

## Not Exposed in Agent REST

- No delete route.
- No create-version route.
- No submit route.

Agent flow is create game + update current version + validate/status.

## Common Error Codes

- `UNAUTHORIZED` - Missing/invalid API key.
- `ACCOUNT_INELIGIBLE` - User is banned/deleted.
- `FORBIDDEN` - User does not own the game/version.
- `VERSION_NOT_FOUND` - Game/version mismatch or missing.
- `MAX_IN_DEV_GAMES_EXCEEDED` - Creator reached cap.
- `VALIDATION_ERROR` - Body/query failed validation.
- `RATE_LIMITED` - API key rate limit exceeded.
