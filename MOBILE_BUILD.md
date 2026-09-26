# Android app

The Android app wraps the existing ShifoUZ React interface with Capacitor.

## Build a debug APK

Requirements: Node.js, Java 21, and Android SDK platform/build-tools. Run:

```powershell
npm run android:apk
```

The APK is written to `android/app/build/outputs/apk/debug/app-debug.apk`. Install it on a connected device or emulator with:

```powershell
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

`npm run android:run` builds and launches on a connected emulator/device. `npm run android:open` opens the native project in Android Studio.

## Authentication networking

For Android Emulator, a configured `http://localhost:3001` auth URL is routed to the host machine at `10.0.2.2`. The debug manifest permits local HTTP only for testing. For a release APK, set `VITE_AUTH_API_URL` in `.env.local` to a publicly reachable HTTPS auth server, rebuild, and do not enable cleartext traffic in the release manifest. A physical phone cannot use the emulator's `10.0.2.2` address; use a reachable LAN IP for local testing or deploy the API over HTTPS.

iOS builds require macOS and Xcode; this Windows workspace is configured for Android.