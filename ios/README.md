# ios/ — VEXVortex native iOS app (landing zone)

Native Swift/SwiftUI client for VEX Robotics stats.

Planned layout (once the Xcode project lands):

```text
ios/
├── VEXVortex.xcodeproj / .xcworkspace
├── VEXVortex/
│   ├── App/            # Entry point, environment, routing
│   ├── Features/       # Teams, Events, Matches, Rankings, Favorites
│   ├── Networking/     # Supabase REST client, DTOs, caching
│   ├── Persistence/    # Favorites + offline cache
│   └── Resources/      # Assets, xcconfig (no secrets)
└── VEXVortexTests/ / VEXVortexUITests/
```

Rules:

- Supabase `anon` key only in the bundle. `service_role` never ships.
- Every stat links to its source; cached rows show stale timestamps.
- Secrets via Xcode `xcconfig` / scheme env locally and Codemagic variables in CI.
