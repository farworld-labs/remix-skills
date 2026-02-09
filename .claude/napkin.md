# Napkin

## Corrections
| Date | Source | What Went Wrong | What To Do Instead |
|------|--------|-----------------|--------------------|
| 2026-02-09 | Self | Tried to read Next.js dynamic route files without quoting bracketed path segments; zsh expanded as glob and failed. | Quote paths containing `[` and `]` (or escape brackets) when using shell tools like `sed` and `cat`. |

## User Preferences
- Keep git remote changes direct: update `origin` when user provides a new repo URL.

## Patterns That Work
- Verify current remotes with `git remote -v` before changing `origin`.
- Quote dynamic route paths containing `[` and `]` when reading Next.js app router files from shell commands.

## Patterns That Don't Work
- None yet.

## Domain Notes
- Repo: `/Users/chuckstock/repos/skills`.

## Session Notes
- 2026-02-08: Updated `origin` from `farworld-labs/skills` to `farworld-labs/remix-skills` and pushed `master` with `-u`; this is the correct flow for repo migration requests.
- 2026-02-09: Reviewed `skills/` coverage against Farcade source (`v1/agents` + `packages/game-sdk`), added `skills/references/game-sdk.md`, and tightened API contract docs/error codes.
