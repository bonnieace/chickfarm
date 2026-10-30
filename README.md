# ChickFarm

ChickFarm is a Flutter/Firebase poultry-management MVP for farm expenses,
workers, flock mortality, vaccination schedules, egg production and egg sales.

## Production data model

- `users/{uid}` stores the user's active tenant.
- `tenants/{tenantId}` stores farm identity.
- `tenants/{tenantId}/members/{uid}` controls farm access.
- Expenses and workers are tenant-scoped.
- Mortalities, vaccinations, egg collections and egg sales are scoped under
  the active flock.
- Sellable egg stock is updated atomically when good eggs are collected or sold.
- Egg sales support trays, loose eggs, buyer details and paid, partial or credit status.
- Stock corrections are append-only and require a reason, preserving an audit trail.

Firestore rules in `firestore.rules` enforce the same boundary. Root-level
legacy collections are intentionally inaccessible to the hardened client.

## Firebase setup

1. Enable Email/Password authentication in Firebase Authentication.
2. Deploy rules and indexes: `firebase deploy --only firestore`.
3. Add an ignored `android/key.properties` and release keystore for signed Android builds.
4. Build with `flutter build appbundle` or `flutter build web`.

Existing prototype records in root collections are not migrated automatically;
copy them into the appropriate tenant/flock only after assigning ownership.

## Verification

```sh
flutter pub get
flutter analyze
flutter test
flutter build appbundle
```

## Standalone repository

ChickFarm is maintained independently from Jengaapp. The standalone baseline is
`bonnieace/Jengaapp`, branch `production-hardening`, commit
`0e5e1510bf99581819d07d8d9ad297a8a9af926c`.
The existing Git history is preserved. New ChickFarm work belongs on `main`
in `bonnieace/chickfarm`; the source Jengaapp branches are preserved.
Firebase remains `chickfarm-uzanet`. Repository creation does not copy GitHub
secrets, environments, deployment integrations, or release signing credentials.
The Android workflow builds a test APK on pushes to `main`.

Brand assets and character guidance live in `assets/brand/` and `docs/BRAND.md`.
