# Research Notes

Last reviewed: 2026-10-23

## 1. Discord Rich Presence on Android

**Status: Android RPC is officially documented; CraftPresence compatibility and SDK package details remain open.**

Discord's current Rich Presence guide says RPC without authentication is supported on desktop and Android starting with Social SDK 1.10+. On Android, Discord must be installed and signed in, and the app must have a registered Application ID. The documented basic flow is to set the Application ID, configure an activity, then call `Client::UpdateRichPresence`. This avoids the usual SDK authentication connection for basic presence.

The official release notes list Android support for unauthenticated RPC in the 1.10 release line. Discord's mobile support announcement describes the SDK as supporting C++, Unreal Engine, and Unity on Android and iOS.

The Social SDK is a C++ library. The installation documentation describes a standalone C++ SDK and engine plugins. A Kotlin-first Android app would likely need a native C++/JNI boundary if the SDK package supports the intended standalone Android integration. This is a design risk, not yet a resolved implementation detail.

### Intended-use uncertainty

Discord's public documentation describes Social SDK integrations primarily in the context of games and their social features. The general Rich Presence overview also refers to a user's game or app, but the documentation reviewed does not explicitly answer whether a separate companion app that detects another game's foreground state is an approved use. The SDK Terms license use only as authorized by the documentation and terms. Do not claim that this use case is approved or prohibited without an authoritative answer from Discord.

**Decision:** Before using or distributing the SDK for CraftPresence, obtain clarification from Discord support/developer support about the standalone companion-app use case. Do not misrepresent CraftPresence as a game to get through a portal form.

### Download and packaging limitation

The official release notes say SDK downloads are available from the Social SDK section of the Developer Portal for a registered application. The public documentation reviewed does not expose a definitive current Android archive manifest listing supported ABIs, exact native library filenames, NDK/CMake versions, and package size. Those details need to be inspected in the authorized portal download itself. Do not guess them or download large build tools until the archive requirements are known.

### Sources

- Setting Rich Presence: https://discord.com/developers/docs/social-sdk/development-guides/setting-rich-presence
- SDK installation: https://discord.com/developers/docs/social-sdk/installation.html
- Social SDK release notes: https://discord.com/developers/docs/social-sdk/release_notes.html
- Social SDK overview: https://discord.com/developers/docs/social-sdk/index.html
- Discord announcement for Social SDK 1.10 mobile support: https://discord.com/blog/mobile-platform-support-arrives-in-discord-social-sdk
- Social SDK Terms: https://support-dev.discord.com/hc/en-us/articles/30225844245271-Discord-Social-SDK-Terms
- Rich Presence overview and SDK choice: https://discord.com/developers/docs/rich-presence/overview

### Remaining questions

- Does Discord authorize a standalone companion app reporting a third-party game's foreground state?
- What Android libraries and ABI variants are in the current portal download?
- What minimum Android API, NDK, CMake, and compiler versions are required?
- What redistribution, attribution, and licensing requirements apply?
- Does the target Discord Android client accept the activity on the tablet?
- What is the supported way to clear activity and handle Discord restarts?

Do not use undocumented private Discord endpoints, impersonate another app, or try to bypass the SDK application process.

## 2. Minecraft Bedrock foreground detection

**Status: Open; no target-device test has been recorded.**

The intended initial feature is deliberately narrow: determine whether the Minecraft app is in the foreground. Android's UsageStatsManager may provide usage events after the user grants Usage Access, but the actual permission settings and event behavior must be verified on the target device and Android version.

The project owner reports that Minecraft appears to have Usage Access on the tablet. This does not grant permission to CraftPresence; CraftPresence must request its own access and the owner must explicitly authorize it. A working test must be performed with CraftPresence installed.

Foreground detection cannot by itself identify a Minecraft world, dimension, server, or game mode. Those fields remain unavailable unless a separate supported bridge is implemented and verified.

Testing needs to cover app switching, screen lock, process death, permission revocation, battery optimization, and reboot. No restriction bypass is in scope.

## 3. Building from Termux on Android

**Status: Open.**

The project owner has Java 21.0.13 and Gradle 9.3.1 available in Termux. clang is installed, CMake and sdkmanager were not found, and both ANDROID_HOME and ANDROID_SDK_ROOT are unset. Approximately 11 GB of free storage was reported at the last check.

This establishes that Java and Gradle can run; it does not establish that a compatible Android SDK, build-tools package, NDK, CMake, or a full APK build path is available.

Before any large downloads:
1. Determine the Android Gradle Plugin and Gradle version pair required by the selected Android project.
2. Determine exact SDK, build-tools, NDK, and CMake versions from the selected SDK's requirements.
3. Check whether the required Android tools can be installed and run in the current Termux environment.
4. Estimate total download and extracted disk usage.
5. Build a minimal app and install it on the tablet before attempting the full project.
6. Build a minimal JNI/native-library sample only if the Discord integration route survives the terms and packaging checks.

## 4. Public repository handling

The project owner has chosen to keep the repository public for now. Public repositories can be read by anyone who finds them. Never commit Discord tokens, OAuth secrets, signing keys, local environment files, account identifiers that should remain private, or raw logs containing sensitive information. The root `.gitignore` excludes common local configuration and build outputs.

## 5. Current conclusion

Official documentation confirms that Android unauthenticated RPC Rich Presence exists in Social SDK 1.10+. It does not yet confirm the suitability of the SDK for this companion-app use case or the exact native package requirements. CraftPresence remains in Phase 0 until intended-use questions, SDK packaging/terms, foreground detection, and on-device build feasibility have been tested. No application code should be written before the Phase 0 exit criteria are met.
