---
name: remix-cli
description: Use the official Remix CLI for authentication, config inspection, game creation, uploads, and analytics. Trigger this skill when a task involves `remix login`, `remix whoami`, `.remix-cli.json`, `REMIX_API_KEY`, or terminal-based game management on Remix.
---

# Remix Cli

## Overview

Use the `remix` CLI for human-facing terminal workflows. Prefer it over handwritten REST calls when the task is local auth, config management, game creation, asset uploads, code uploads, or analytics inspection.

## Quick Start

Install and authenticate:

```bash
curl -fsSL https://remix.gg/install.sh | bash
remix login
remix whoami
```

Create and upload a game:

```bash
remix games create --name "My Game"
remix games icon upload ./icon.png
remix games versions code update --code-path ./game.html
remix games versions validate get
```

## Output Modes

- `--json` forces machine-readable output.
- non-TTY stdout defaults to JSON.
- `--quiet` suppresses human-only status output and implies JSON-friendly output.

Use JSON mode for agent parsing and shell pipelines.

## Config Model

- Project config lives in the nearest `.remix-cli.json`.
- Credentials live in `~/.config/remix/credentials.json`.
- Legacy `.remix-mcp.json` can still be discovered, but new CLI config is `.remix-cli.json`.
- Runtime precedence is:
  1. explicit flags
  2. environment variables
  3. stored config and credentials
  4. defaults

Useful inspection commands:

```bash
remix config get
remix config where
remix whoami --json
```

## Commands To Reach For

- Auth: `remix login`, `remix whoami`
- Config: `remix config set|get|where`
- Health: `remix health get`
- Games: `remix games list|get|create`
- Assets: `remix games assets list|upload`, `remix games icon upload`
- Metadata: `remix metadata categories list`, `remix games categories set`
- Versions: `remix games versions list|get|code get|code update|thread get|status get|validate get`
- Readiness: `remix games launch-readiness get`
- Analytics: `remix games analytics overview get`, `remix games analytics shop get`

## Guardrails

- Reuse `gameId` and `versionId` from `.remix-cli.json` when present.
- Run `remix whoami` before debugging auth manually.
- Use `remix games list` only when the user explicitly wants their game list.
- Prefer `remix games versions code update --code-path ...` over constructing raw upload requests by hand.
- For local development against a non-production API, set `remix config set --api-url http://localhost:3003`.
