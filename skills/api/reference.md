---
name: api-reference
description: API reference for Remix agent publishing REST routes
metadata:
  tags: remix, api, rest, reference
---

## Transport

Base URL: `https://api.remix.gg`

Primary agent publishing endpoints are exposed under `/v1/agents`.

Compatibility aliases are also available under `/api/v1/agents`.

If this reference and runtime behavior differ, use OpenAPI as source of truth:
- `https://api.remix.gg/openapi`
- `https://api.remix.gg/openapi/json`

Base headers:

```http
Authorization: Bearer <api_key>
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

### `GET /v1/agents/metadata/categories`
List supported game category enums.

Output (`200`):

```json
{
  "success": true,
  "data": {
    "categories": ["ACTION", "ARCADE", "..."]
  }
}
```

### `GET /v1/agents/games`
List games owned by the authenticated user.

Output (`200`) includes:
- `id`, `name`, `appImageUrl`, `createdAt`, `updatedAt`
- `liveVersionId`, `latestVersionId`, `latestVersionCreatedAt`
- computed `status` for the latest version

### `GET /v1/agents/games/{gameId}`
Get owned game detail.

Output (`200`) includes:
- `id`, `name`, `appImageUrl`, timestamps
- `liveVersionId`, `threadId`, `hasThread`
- `categories[]`

Errors:
- `GAME_NOT_FOUND`
- `FORBIDDEN`

### `GET /v1/agents/games/{gameId}/versions`
List all versions for an owned game.

Output (`200`) includes:
- `gameId`, `liveVersionId`
- `versions[]` with `id`, `title`, timestamps, `feedback`, and computed `status`

Errors:
- `GAME_NOT_FOUND`
- `FORBIDDEN`

### `GET /v1/agents/games/{gameId}/versions/{versionId}`
Get detailed view for an owned version.

Output (`200`) includes:
- `id`, `gameId`, `title`, timestamps
- `status`, `feedback`
- `sdkEventStates`, `passedSdkCheck`
- `playtestStatus`, `isSafeToPublish`
- `liveVersionId`

Errors:
- `VERSION_NOT_FOUND`
- `FORBIDDEN`

### `GET /v1/agents/games/{gameId}/versions/{versionId}/code`
Get stored code for an owned version.

Output (`200`):

```json
{
  "success": true,
  "data": {
    "gameId": "...",
    "versionId": "...",
    "code": "<html>...</html>"
  }
}
```

### `GET /v1/agents/games/{gameId}/versions/{versionId}/thread`
Get thread context for an owned version.

Output (`200`):

```json
{
  "success": true,
  "data": {
    "gameId": "...",
    "versionId": "...",
    "threadId": "thread_xxx",
    "html": "<html>...</html>"
  }
}
```

Notes:
- `threadId` may come from version or game-level thread.
- `html` falls back to thread HTML when version code is empty.

### `GET /v1/agents/games/{gameId}/assets`
List non-deleted assets for an owned game.

Output (`200`) includes `assets[]` with:
- `id`, `name`, `url`, `contentType`, timestamps

Errors:
- `GAME_NOT_FOUND`
- `FORBIDDEN`

### `GET /v1/agents/games/{gameId}/launch-readiness`
Get aggregate launch readiness for a game version.

Query:
- optional `versionId` (when omitted, server uses latest version)

Output (`200`) includes:
- `gameId`, `versionId`
- `status`
- `valid`
- `blockers[]`
- `readyForSubmission` (`valid && status === "draft"`)

### `POST /v1/agents/games`
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
- `name` is required and must be `5-25` chars.

### `PUT /v1/agents/games/{gameId}/versions/{versionId}/code`
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
- Code size must be between `1` and `1_000_000` characters.
- Updates existing version only.
- Cannot update live version.
- Cannot update submitted/launched/approved versions.
- Resets publish fields (`submittedAt`, `approvedAt`, `launchedAt`, `feedback`) and SDK checks after write.
- Creates a thread when missing and uploads code to agent-builder.

### `GET /v1/agents/games/{gameId}/versions/{versionId}/validate`
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

Validation checks currently include:

- `MISSING_CODE`
- `MISSING_NAME`
- `MISSING_ICON`
- `MISSING_CATEGORY`
- `MISSING_SDK_GAME_OVER`
- `MISSING_SDK_PLAY_AGAIN`
- `MISSING_SDK_TOGGLE_MUTE`

### `GET /v1/agents/games/{gameId}/versions/{versionId}/status`
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

Status mapping is server-derived:

- `live` when `launchedAt` exists
- `approved` when `approvedAt` exists
- `review` when `submittedAt` exists
- `blocked` when `feedback` exists
- `draft` otherwise

## Not Exposed in Agent REST

- No delete route.
- No create-version route.
- No submit route.
- No write endpoints for game metadata or assets in this surface.

Agent flow is create game + update current version + validate/status.

## Common Error Codes

- `UNAUTHORIZED` - Missing/invalid API key.
- `ACCOUNT_INELIGIBLE` - User is banned/deleted.
- `FORBIDDEN` - User does not own the game/version.
- `GAME_NOT_FOUND` - Game missing.
- `VERSION_NOT_FOUND` - Game/version mismatch or missing.
- `MAX_IN_DEV_GAMES_EXCEEDED` - Creator reached cap.
- `VALIDATION_ERROR` - Body/query failed validation.
- `INVALID_JSON` - Body could not be parsed as JSON.
- `LIVE_VERSION_LOCKED` - Attempted to edit a live version.
- `VERSION_NOT_EDITABLE` - Version is submitted/approved/launched.
- `THREAD_CREATE_FAILED` - Failed to provision upload thread.
- `RATE_LIMITED` - API key rate limit exceeded.
- `INTERNAL_SERVER_ERROR` - Unexpected server failure.
