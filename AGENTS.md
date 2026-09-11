# VEXVortex Agent Instructions

## Mission

VEXVortex is a VEX Robotics stats application: native iOS client, Supabase backend (Postgres + RLS, Auth, REST/Data API, Edge Functions, Storage), shipped with Codemagic. Self-hosting / homelab appliance work is a separate project and does not live in this repo.

## Non-negotiable safety rules

- Never place Supabase `service_role`, database passwords, Apple signing keys/profiles, App Store Connect API keys, or Codemagic secrets in Git, client code, logs, screenshots, or public environment variables.
- Never trust `Origin`, `Referer`, User-Agent, client IP, or an app name as proof that a request came from an approved application.
- Preserve RLS as a defense boundary. Do not solve authorization problems with broad grants or `SECURITY DEFINER` functions.
- Every destructive action—deleting data, resetting volumes, rotating credentials, applying migrations, or changing auth policies—must require an explicit confirmation and have a documented rollback/backup path.
- Security tests must be non-destructive, rate-limited, scoped to this project and its containers, and must not scan unrelated home-network devices.

## Working method

1. Inspect the repository, host, and current configuration before changing anything.
2. State assumptions and make a plan for risky changes.
3. Pin versions and commit lockfiles. Verify current upstream documentation before relying on version-specific commands.
4. Make small reversible changes. Prefer `apply_patch` for edits.
5. Run formatting, unit tests, configuration validation, health checks, and security checks appropriate to the change.
6. Document the exact commands, expected results, failure recovery, and remaining risks.

## Architecture defaults

- Client: native iOS (Swift/SwiftUI) in `ios/`. No secrets beyond the Supabase `anon` key in the bundle; `service_role` never ships.
- Backend: managed Supabase — Postgres + RLS, Auth, REST/Data API, Edge Functions, Storage. Migrations and policies live in `supabase/`.
- Ship: Codemagic via `codemagic.yaml` — signed iOS builds, TestFlight/App Store tracks, secrets in Codemagic variables only.
- Data: upstream VEX event data ingested server-side (Edge Functions/jobs), validated, served read-mostly to clients with cached/offline reads and stale indicators.

## Supabase-specific rules

- Enable RLS on every table in an exposed schema and write policies for the actual ownership/tenant model.
- Use `TO authenticated`/`TO anon` policy clauses instead of deprecated `auth.role()` checks.
- UPDATE policies need both `USING` and `WITH CHECK`; UPDATE also needs a SELECT policy.
- Never use editable `user_metadata` for authorization. Use server-controlled claims or database authorization data.
- Keep privileged functions in a private schema, minimize grants, and avoid `SECURITY DEFINER` unless its threat model is documented.
- Do not expose service-role credentials to any client.

## Definition of done

Stats screens show no fake data, every number links to its source, offline/cached reads carry stale indicators, RLS negative cases pass, Codemagic dev build is green, and secrets appear in none of the artifacts.
