---
name: api-reference
description: API reference for Remix agent publishing procedures
metadata:
  tags: remix, api, trpc, reference
---

## Transport

Current endpoints are exposed via tRPC procedures.

- Agent publish procedures: `agentPublish.*`
- Supporting metadata procedures: `game.*`

## Procedures

### `agentPublish.createDraft`
Create a new game draft and initial version.

Input:
```json
{ "name": "Neon Dash" }
```

Output:
```json
{ "gameId": "...", "versionId": "..." }
```

### `agentPublish.createVersion`
Create a new draft version for an existing game.

Input:
```json
{ "gameId": "...", "basedOnVersionId": "optional" }
```

Output:
```json
{ "gameId": "...", "versionId": "..." }
```

### `agentPublish.uploadCode`
Upload/update HTML game code for a version.

Input:
```json
{ "gameId": "...", "versionId": "...", "code": "<html>...</html>" }
```

Output:
```json
{ "success": true, "threadId": "..." }
```

### `agentPublish.validate`
Returns machine-readable blockers for submit readiness.

Input:
```json
{ "gameId": "...", "versionId": "..." }
```

Output:
```json
{
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
```

### `agentPublish.submit`
Submit a version for review.

Requirements:
- `Idempotency-Key` header required

Input:
```json
{
  "gameId": "...",
  "versionId": "...",
  "gameJamId": null,
  "releaseNotes": "optional",
  "releaseTypes": {
    "isBugFix": false,
    "isNewFeature": true,
    "isNewGame": false
  }
}
```

Output:
```json
{
  "gameId": "...",
  "versionId": "...",
  "status": "review",
  "submittedAt": "2026-02-07T..."
}
```

### `agentPublish.status`
Get publishing status for a version.

Input:
```json
{ "gameId": "...", "versionId": "..." }
```

Output:
```json
{
  "gameId": "...",
  "versionId": "...",
  "status": "draft|blocked|review|approved|live",
  "submittedAt": null,
  "approvedAt": null,
  "launchedAt": null,
  "feedback": null
}
```

## Metadata Procedures Required for Submit Readiness

### `game.updateName`
Set game name.

Input:
```json
{ "gameId": "...", "name": "Neon Dash" }
```

### `game.addCategory`
Add category (1-3 required overall).

Input:
```json
{ "gameId": "...", "category": "ARCADE" }
```

### Icon (current limitation)

Submit readiness requires an icon, but there is no dedicated `agentPublish` icon update procedure yet.
Current path is Remix Studio UI/internal upload flows. If icon is missing, `agentPublish.validate` returns blockers.

## Common Errors

- `UNAUTHORIZED` - Missing/invalid API key or session.
- `FORBIDDEN` - User does not own the game/version.
- `NOT_FOUND` - Game/version does not exist or mismatch.
- `BAD_REQUEST` - Validation failure or missing idempotency key on submit.
- `TOO_MANY_REQUESTS` - API key rate limit exceeded.
