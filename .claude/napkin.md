# Napkin

## Corrections
| Date | Source | What Went Wrong | What To Do Instead |
|------|--------|-----------------|--------------------|
| 2026-02-09 | Self | Tried to read Next.js dynamic route files without quoting bracketed path segments; zsh expanded as glob and failed. | Quote paths containing `[` and `]` (or escape brackets) when using shell tools like `sed` and `cat`. |
| 2026-02-09 | Self | Renamed a skill `name` field during docs migration, which can break existing references/triggers. | Keep skill `name` stable unless explicitly requested; update body/docs without renaming identifiers. |
| 2026-02-09 | Self | Assumed `apps/server-api/src/routes/agents.ts` still existed after layout change and initial reads failed. | Re-discover file paths with `rg --files` before editing docs when service layout changes. |
| 2026-02-09 | Self | Tried to validate live API docs via `curl https://api.remix.gg/...`; DNS resolution failed in this environment. | When external DNS is unavailable, verify changes from local repo history/docs and clearly mark date-scoped guidance. |

## User Preferences
- Keep git remote changes direct: update `origin` when user provides a new repo URL.
- Keep explicit API key onboarding in skills docs: users must log in and create keys at `https://remix.gg/api-keys`.
- Keep new skills in this repo (no separate skills repo by default).

## Patterns That Work
- Verify current remotes with `git remote -v` before changing `origin`.
- Quote dynamic route paths containing `[` and `]` when reading Next.js app router files from shell commands.
- For Remix API doc updates, read `apps/server-api/routes/agents.ts` and `https://api.remix.gg/docs` first, then patch all skill references in one pass.
- When `server-api` adds routes, update both core reference docs and the quickstart/discovery sections so agents can actually use the new read endpoints.
- For framework companion skills, keep a consistent layout under `skills/frameworks/<skill-name>/` with `SKILL.md`, `references/`, and `assets/`.
- Keep local API docs concise and OpenAPI-first; avoid duplicating full endpoint contracts that drift.

## Patterns That Don't Work
- None yet.
- Attempting to write under `.agents/skills` in this workspace fails with "Operation not permitted"; use `skills/` for new in-repo skills.

## Domain Notes
- Repo: `/Users/chuckstock/repos/skills`.

## Session Notes
- 2026-02-08: Updated `origin` from `farworld-labs/skills` to `farworld-labs/remix-skills` and pushed `master` with `-u`; this is the correct flow for repo migration requests.
- 2026-02-09: Reviewed `skills/` coverage against Farcade source (`v1/agents` + `packages/game-sdk`), added `skills/references/game-sdk.md`, and tightened API contract docs/error codes.
- 2026-02-09: Migrated skill docs from studio route sources to `apps/server-api` sources; updated primary base URL to `https://api.remix.gg`, primary routes to `/v1/agents/*` (with `/api/v1/agents/*` compatibility note), and auth wording to bearer API token from the web app.
- 2026-02-09: Restored explicit key issuance guidance (`https://remix.gg/api-keys`) after token-wording migration to avoid dropping user onboarding steps.
- 2026-02-09: Expanded skills for new server-api routes: game/version list/detail, code/thread fetch, assets list, launch-readiness, and metadata categories.
- 2026-02-09: Added `skills/frameworks/phaser-2d-arcade` companion skill with references and a starter single-file template; linked from top-level README and Remix skill index.
- 2026-02-09: Added `skills/frameworks/threejs-lite` companion skill with mobile/perf references and single-file starter template; linked from top-level README and Remix skill index.
- 2026-02-09: Synced skills to URL updates in `apps/server-api/routes/agents.ts`: added `gameUrl`/`versionUrl` family fields, switched code upload docs to `POST /v1/agents/games/{gameId}/versions/{versionId}/code`, and updated OpenAPI fallback links to `/docs`.
- 2026-02-09: Refactored skills to OpenAPI-first workflow: fetch `https://api.remix.gg/docs/json` before generating calls, and slimmed `skills/api/reference.md` to stable guardrails.
