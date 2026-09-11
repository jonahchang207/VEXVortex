# supabase/ — VEXVortex backend (landing zone)

Managed Supabase backend: Postgres + RLS, Auth, REST/Data API, Edge Functions, Storage.

Planned layout:

```text
supabase/
├── migrations/     # Versioned schema + RLS policies (vex domain)
├── functions/      # Edge Functions: upstream sync, aggregates
├── seed.sql        # Dev seed (no real secrets)
└── config.toml     # Supabase CLI config
```

Rules (see root `AGENTS.md`):

- RLS on every exposed table; `TO anon`/`TO authenticated`, never `auth.role()`.
- UPDATE needs `USING` + `WITH CHECK` plus a SELECT policy.
- No editable `user_metadata` auth; no `SECURITY DEFINER` without a documented threat model.
- Upstream VEX data is ingested + validated server-side, served read-mostly.
