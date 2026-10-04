# Moonlight Fold

Moonlight Fold is an experimental Android client for using Samsung Galaxy Z Fold devices as full-screen Moonlight displays.

It is a small fork of [Moonlight Android](https://github.com/moonlight-stream/moonlight-android). The first fix prevents Android's forced landscape handling from creating a portrait-oriented video surface on foldable hardware.

## The problem

On a Galaxy Z Fold 8, official Moonlight 12.2 opened a full-screen 2448x1848 activity but Android exposed the stream content surface as 1848x2448. The resulting native-resolution stream was scaled down and surrounded by large black borders.

Moonlight's stretch option and Samsung's per-app aspect-ratio setting did not correct the transposed surface.

## The fix

On devices that advertise `android.hardware.sensor.hinge_angle`, Moonlight Fold uses `SCREEN_ORIENTATION_FULL_USER` instead of forcing `SCREEN_ORIENTATION_USER_LANDSCAPE`.

This was verified on a Samsung Galaxy Z Fold 8 (`SM-F971U1`) with:

- Android 17
- native 2448x1848 streaming
- 60 FPS HEVC from Sunshine
- a matching BetterDisplay virtual display on macOS

The stream fills the unfolded inner display edge to edge. Non-foldable devices retain Moonlight's existing orientation behavior.

The fix has also been proposed upstream in [moonlight-stream/moonlight-android#1613](https://github.com/moonlight-stream/moonlight-android/pull/1613).

## Status

This repository is public for testing and review. It uses distinct application IDs and the **Moonlight Fold** label so development builds can coexist with official Moonlight.

There is not yet a stable public APK release. Until release signing and automated builds are ready, build from source using the instructions below.

## Building

1. Install Android Studio and Android NDK `29.0.14206865`.
2. Clone recursively:

   ```sh
   git clone --recursive https://github.com/sblocktechnologies/moonlight-fold.git
   cd moonlight-fold
   ```

3. Create `local.properties` with your SDK and NDK paths:

   ```properties
   sdk.dir=/path/to/Android/sdk
   ndk.dir=/path/to/Android/sdk/ndk/29.0.14206865
   ```

4. Build the non-root debug APK:

   ```sh
   ./gradlew assembleNonRootDebug
   ```

The APK will be under `app/build/outputs/apk/nonRoot/debug/`.

## Upstream project

Moonlight Android is an open-source client for NVIDIA GameStream and [Sunshine](https://github.com/LizardByte/Sunshine). For general Moonlight downloads, support, and documentation, use the [official Moonlight project](https://moonlight-stream.org).

## License

Moonlight Fold retains Moonlight Android's GNU GPL v3 license. See [LICENSE.txt](LICENSE.txt).
