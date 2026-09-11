# VEXVortex Agent Instructions

## Mission

VEXVortex is a self-hosted backend appliance for personal applications. It runs on a wiped 2019 Intel MacBook Air with Debian 13 stable, Docker Compose, self-hosted Supabase-compatible services, Tailscale-only administration, and a local fullscreen operations display.

## Non-negotiable safety rules

- Do not expose PostgreSQL, Docker, Supabase Studio, the admin app, SSH, or Tailscale administration to the public internet.
- Do not require router port forwarding. Prefer outbound-only connectivity through Tailscale and, only when needed, Cloudflare Tunnel or a narrowly scoped public HTTPS endpoint.
- Never trust `Origin`, `Referer`, User-Agent, client IP, or an app name as proof that a request came from an approved application.
- Never place Supabase `service_role`, database passwords, tunnel tokens, SSH private keys, or Tailscale auth keys in Git, browser code, logs, screenshots, or public environment variables.
- Preserve RLS as a defense boundary. Do not solve authorization problems with broad grants or `SECURITY DEFINER` functions.
- Every destructive action—wiping disks, deleting data, resetting volumes, rotating credentials, applying migrations, or changing firewall rules—must require an explicit confirmation and have a documented rollback/backup path.
- Security tests must be non-destructive, rate-limited, scoped to this server and its containers, and must not scan unrelated home-network devices.

## Working method

1. Inspect the repository, host, and current configuration before changing anything.
2. State assumptions and make a plan for risky changes.
3. Pin versions and commit lockfiles. Verify current upstream documentation before relying on version-specific commands.
4. Make small reversible changes. Prefer `apply_patch` for edits.
5. Run formatting, unit tests, configuration validation, health checks, and security checks appropriate to the change.
6. Document the exact commands, expected results, failure recovery, and remaining risks.

## Architecture defaults

- OS: Debian 13 stable (`amd64`), minimal install, encrypted disk where practical.
- Runtime: Docker Engine/Compose with pinned image tags; rootless Docker is preferred when compatible with the required services.
- Data: internal SSD for live data; encrypted rotating backups on the 250 GB flash drive. Treat the flash drive as a backup target, not the only copy.
- Network: default-deny host firewall; Tailscale for admin access; public access only to explicitly required HTTPS application routes.
- Supabase: use the official self-hosted Docker Compose distribution, pinned to a known release. Remember that self-hosted Supabase is a single project and does not provide the managed platform's branching, managed backups, or platform API.
- Admin: custom Tailscale-only web app; SSH remains the only terminal access path.
- Display: dedicated fullscreen local dashboard with no browser chrome, no desktop workflow, and automatic restart.

## Supabase-specific rules

- Enable RLS on every table in an exposed schema and write policies for the actual ownership/tenant model.
- Use `TO authenticated`/`TO anon` policy clauses instead of deprecated `auth.role()` checks.
- UPDATE policies need both `USING` and `WITH CHECK`; UPDATE also needs a SELECT policy.
- Never use editable `user_metadata` for authorization. Use server-controlled claims or database authorization data.
- Keep privileged functions in a private schema, minimize grants, and avoid `SECURITY DEFINER` unless its threat model is documented.
- Do not expose service-role credentials to any client.

## Definition of done

The system must survive reboot, temporary network loss, Docker service failure, and a failed backup without silently losing data. It must provide tested restore procedures, visible health status, auditable admin actions, and a clear migration path from the user's managed Supabase projects.
