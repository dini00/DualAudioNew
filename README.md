# Dual Audio New (Android Studio source project)

This is a **new starter implementation** for Android 10+ that:
- lists currently connected wired/USB and Bluetooth audio outputs;
- asks the user for MediaProjection permission;
- captures other apps' playback when Android and the source app permit playback capture;
- copies captured PCM audio into two separate AudioTrack instances and requests different output devices.

## Important limitation
This project is source code, **not a tested APK**. Android's `setPreferredDevice()` is a routing preference, not a guarantee. Some phone/OEM audio policies may route both tracks to the same output or reject simultaneous routing. This app cannot override system-level routing restrictions without appropriate OS/OEM support. Audio capture is also blocked by apps that opt out of playback capture, and DRM-protected content may not be capturable.

## Open and build
1. Install Android Studio with Android SDK Platform 35 and a compatible JDK.
2. Extract this ZIP.
3. Open the `DualAudioNew` folder in Android Studio.
4. Let Gradle sync, then build/install the `app` module.
5. On the phone, connect USB-C wired earphones and Bluetooth earbuds.
6. Open the app, press **Refresh devices**, select one device in each list, and press **Start Dual Audio**.
7. Accept Android's screen/audio capture permission prompt.
8. Play audio in another app. Stop using the app's **Stop Dual Audio** button.

## Diagnostics
Use Android Studio Logcat and filter by `DualAudioNew`. It logs the selected device IDs, whether the preferred-device request was accepted, and the actual `routedDevice` values. A successful preference request does not itself prove that the phone is routing the track to that device.

## Current scope
This starter uses a simple programmatic UI and an AudioPlaybackCapture -> AudioRecord -> two AudioTrack pipeline. It does not include a custom video player, background audio mixing, latency compensation, or OEM-specific privileged routing.


## Build APK using only your phone (GitHub Actions)
This ZIP includes `.github/workflows/build-apk.yml`.
1. On your phone, open GitHub in Chrome and sign in.
2. Create a **new empty repository** (for example `DualAudioNew`).
3. Upload the contents of this ZIP to the repository, keeping the folder structure. Do not upload the ZIP file itself as the only repository file.
4. Open the repository's **Actions** tab and enable Actions if GitHub asks.
5. Choose **Build Dual Audio APK** and press **Run workflow** (or push/upload to `main` to trigger it).
6. Open the completed workflow run, scroll to **Artifacts**, and download `DualAudioNew-debug-apk`.
7. Extract the downloaded artifact ZIP; it contains `app-debug.apk`. Open it on the phone and allow installation from that source if Android asks.

A cloud build can still fail if the project or Android SDK has an issue. The APK is a debug build, not a Play Store release. Actual dual-output routing must be tested on the target phone.
