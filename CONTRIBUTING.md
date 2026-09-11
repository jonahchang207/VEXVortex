# Contributing — VEXVortex

VEXVortex is a single-maintainer project by **Jonah Chang (@jonahchang207)**. History is kept clean with one author.

## How to help

1. Open an issue first: what, why, rollback plan, expected test evidence.
2. Keep changes small, declarative, version-pinned, and reversible.
3. Read `AGENTS.md` plus the nested `AGENTS.md` for the area you touch (`apps/admin`, `infra`, `docs`, `security`).
4. Never include secrets, tokens, private IPs, or personal identifiers.

## Pull requests

- Issues and ideas are welcome. PRs may be closed in favor of a maintainer-authored commit to preserve single-author history.
- If a PR is accepted, it will be rebased/squashed and committed as the maintainer.
- No `Co-authored-by` trailers unless the maintainer explicitly requests them.

## Checks

- No secrets in diff (`service_role`, DB passwords, tunnel tokens, SSH keys, Tailscale keys).
- Docs updated with exact commands, expected output, and recovery steps.
- Version pins + lockfiles preserved.
