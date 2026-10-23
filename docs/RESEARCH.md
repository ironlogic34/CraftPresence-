# Research Notes

Last reviewed: 2026-10-23

## 1. Discord Rich Presence on Android

**Status: Official documentation supports the concept; target-device proof of concept is still required.**

Discord's Social SDK documentation says Rich Presence via RPC is supported on desktop and Android from Social SDK 1.10+. The Android client must be installed and signed in, and the application needs a registered Discord Application ID. This mode does not require the usual SDK authentication connection.

The SDK is a C++ library. That means a Kotlin-first Android app may need a native library/JNI boundary, depending on the official package and supported integration pattern. This is a design risk, not yet a resolved implementation detail.

Sources:
- Setting Rich Presence: https://discord.com/developers/docs/social-sdk/development-guides/setting-rich-presence
- Social SDK release notes: https://discord.com/developers/docs/social-sdk/release_notes.html
- Social SDK overview: https://discord.com/developers/docs/social-sdk/index.html
- Discord announcement for Social SDK 1.10 mobile support: https://discord.com/blog/mobile-platform-support-arrives-in-discord-social-sdk

Open questions:
- What exact Android libraries and ABI variants are provided by the current SDK download?
- What minimum Android/API and NDK versions are required for this path?
- Can the SDK be used by a companion app that is not itself a game, under the applicable terms?
- What are the distribution, attribution, and licensing requirements?
- Does the target Discord Android app accept the test activity as expected on the tablet?
- What is the supported way to clear activity and handle Discord restarts?

Do not use undocumented private Discord endpoints or impersonate another app. Use the official supported SDK route unless evidence proves it unsuitable.

## 2. Minecraft Bedrock foreground detection

**Status: Open; no target-device test has been recorded.**

The intended initial feature is deliberately narrow: determine whether the Minecraft app is in the foreground. Android's UsageStatsManager may provide usage events after the user grants Usage Access, but the actual permission settings and event behavior must be verified on the target device and Android version.

The prior attempt to find Termux in the Usage Access list returned no output. This does not prove the permission is unavailable, nor does it prove CraftPresence will be listed. The test must be repeated with a purpose-built app installed on the device.

Foreground detection cannot by itself identify a Minecraft world, dimension, server, or game mode. Those fields remain unavailable unless a separate supported bridge is implemented and verified.

Testing needs to cover app switching, screen lock, process death, permission revocation, battery optimization, and reboot. No restriction bypass is in scope.

## 3. Building from Termux on Android

**Status: Open.**

The project owner has Java 21.0.13 and Gradle 9.3.1 available in Termux. This only establishes that Java and Gradle can run; it does not establish that a compatible Android SDK, build-tools package, NDK, CMake, or a full APK build path is available.

Earlier checks did not find Android SDK paths and CMake was not installed. clang was present and approximately 11 GB of storage was free at that time. Recheck before installing anything, because available storage and package compatibility can change.

Before any large downloads:
1. Determine the Android Gradle Plugin and Gradle version pair required by the selected Android project.
2. Determine exact SDK, build-tools, NDK, and CMake versions from the selected SDK's requirements.
3. Check whether the required Android tools can be installed and run in the current Termux environment.
4. Estimate total download and extracted disk usage.
5. Build a minimal app and install it on the tablet before attempting the full project.

## 4. Public repository handling

The project owner has chosen to keep the repository public for now. Public repositories can be read by anyone who finds them. Never commit Discord tokens, OAuth secrets, signing keys, local environment files, account identifiers that should remain private, or raw logs containing sensitive information. Add a suitable .gitignore before local development and use test accounts where practical.

## 5. Current conclusion

The official Discord documentation makes the Rich Presence path plausible, but it does not prove the full CraftPresence design will work. The project remains in Phase 0 until SDK packaging and terms, Android foreground detection, and on-device build feasibility have been tested. No application code should be written before the Phase 0 exit criteria are met.
