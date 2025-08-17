
# One-Click APK via GitHub Actions

This pack lets you generate an **Android APK** from the Flutter app inside your repo, automatically on every push to `main` or on-demand.

## Folder Structure Required

Place your Flutter app in `app/` and your backend in `server/` (optional). The workflow assumes the Flutter project `pubspec.yaml` is at `app/pubspec.yaml`.

```
your-repo/
  app/            # Flutter app (from the canvas code I gave you)
    pubspec.yaml
    lib/
    android/
    ios/
  server/         # Node.js API (optional for APK build)
  .github/
    workflows/
      flutter-android.yml
```

## Steps

1. Create a **new GitHub repository** (or open your existing one).
2. Add your project files (copy the `app/` and `server/` folders from the provided code).
3. Copy the `.github/workflows/flutter-android.yml` file from this pack into your repo in the same path.
4. Commit & push to `main`.
5. Go to **GitHub → Your Repo → Actions → Build Android APK (Flutter)**.
6. Wait for it to finish. Then open the **Artifacts** section and **download** the APK:
   - `debug-apk/app-debug.apk` (installable without signing; good for testing)
   - `release-unsigned-apk/app-release.apk` (for store/upload; needs signing)

> If you don't have the full app yet, add the app code I provided in the chat canvas under `app/`.

## Local Build (optional)

To build on your own computer:

```bash
cd app
flutter pub get
flutter build apk --debug
# artifact: app/build/app/outputs/flutter-apk/app-debug.apk
```

For release:
```bash
flutter build apk --release
```

Then sign the release APK (see `android-keystore-setup.md`).

## Troubleshooting

- Ensure Flutter SDK stable channel works locally: `flutter doctor`.
- On CI, the workflow handles setup automatically.
- If your backend is needed for login, run it separately; APK build does not require backend running.
