# Security Policy — VEXVortex

Maintainer: Jonah Chang ([@jonahchang207](https://github.com/jonahchang207))

## Supported

Single-maintainer personal app project. Security fixes are prioritized over features.

## Do NOT

- Do not post `service_role` keys, database passwords, tunnel tokens, SSH private keys, Tailscale auth keys, private IPs, or screenshots containing secrets in issues, PRs, or discussions.
- Do not scan, brute-force, DoS, or pivot beyond what is explicitly in scope.

## Report a vulnerability

Open a private GitHub Security Advisory for this repo, or contact the maintainer via GitHub. Include:

1. Affected file/route/commit
2. Impact + preconditions (no exploit payloads that destroy data)
3. Safe reproduction steps
4. Suggested remediation + rollback

## Scope

In scope: this repository, its containers when you run them locally, and explicitly listed public HTTPS app routes (if any).

Out of scope: unrelated LAN devices, third-party services, social engineering, physical theft scenarios beyond the documented threat model.

## Hardening promises

- RLS on every exposed table; least-privilege credentials; service-role never in clients.
- Tailscale-only admin; SSH is the only terminal; no public Postgres/Studio/Docker/SSH.
- Non-destructive, rate-limited testing only.
