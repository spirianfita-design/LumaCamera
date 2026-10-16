# Luma Camera (Android)

A native Android Studio project for a real CameraX-based camera application. It uses the device camera pipeline rather than a simulated preview.

## Included
- Live hardware preview, front/rear switching, tap-to-focus and auto-exposure metering.
- Low-latency photo capture saved to `Pictures/LumaCamera`.
- Video recording saved to `Movies/LumaCamera`, with microphone audio when permission is granted.
- Pinch zoom and quick zoom presets constrained to the selected camera's reported zoom range.
- Flash/torch control when the selected camera reports flash capability.
- Exposure compensation slider constrained by the active camera's supported range.
- Photo timer (3/10 seconds), thirds grid, aspect/framing toggle, video quality selector (HD/FHD/4K with CameraX quality fallback).
- Device capability panel showing camera count, facing, zoom, flash, exposure range, FPS ranges, autofocus, stabilization and Camera2 hardware level.
- Gallery grid with open, share/export and delete actions for app-created media.
- Clear messages for modes requiring unavailable or not-yet-implemented specialized processing.

## Build
1. Install Android Studio (current stable) and Android SDK Platform 35.
2. Open this directory as a project.
3. Let Gradle sync and install to a physical Android device.
4. Grant Camera and Microphone permissions when prompted. The microphone is requested only when starting video recording.

## Important capability notes
- This is an Android app, not an iOS app. iOS requires a separate Swift/AVFoundation implementation.
- CameraX and Camera2 expose different capabilities on different manufacturers. The app queries the selected camera rather than promising features it cannot verify.
- Photo and video, zoom, focus/metering, exposure compensation and media saving are implemented. `Portrait`, true high-frame-rate slow motion, interval time-lapse, panorama stitching, hardware macro switching, manual focus, RAW capture, Live Photo, and synthetic cinematic depth blur are intentionally not faked; the UI explains the unavailable advanced modes.
- The aspect-ratio toggle changes the preview framing indicator but currently saves sensor-native 4:3/maximum output according to CameraX. It is not a post-capture crop.
- The CameraX quality selector requests the selected quality and uses a supported fallback if the device cannot provide it. Actual output quality depends on device capabilities.
- No build or device instrumentation tests were run in this environment. Test on the target handset before publishing.

## Suggested production release work
- Add automated instrumentation tests on a physical-device matrix (front/back, rotation, incoming call, low storage, permission revocation, supported resolutions/FPS).
- Add Play policy privacy disclosures and a privacy policy before public distribution.
- Add explicit metadata/location controls, persistent settings, robust Android 14+ media deletion permission flow, RAW/HDR processing where supported, and device-specific validation.

## Build an APK using GitHub Actions (no local Android SDK needed)
1. Create a GitHub repository and upload the contents of this project to it.
2. Ensure the default branch is named `main` or `master`.
3. Open the repository's **Actions** tab and select **Build Luma Camera APK**.
4. Select **Run workflow** (or push a commit to `main`/`master`) and wait for the workflow to finish successfully.
5. Open the completed workflow run and download the `LumaCamera-debug-apk` artifact. Extract the downloaded artifact ZIP to get `app-debug.apk`.
6. Transfer `app-debug.apk` to your Android phone and open it to install. If Android asks, allow installation from that source.

This workflow produces a debug-signed APK for personal testing, not a Play Store release. The source project has not been compiled or device-tested in this environment; the workflow is provided so a real Android build environment can compile it and report any build errors.
