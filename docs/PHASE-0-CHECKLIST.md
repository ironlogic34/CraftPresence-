# Phase 0: Feasibility Checklist

**Rule:** do not begin application implementation until the required gates below have evidence. A document or web page is research evidence, not proof that the feature works on the target tablet.

Status labels: **Verified in documentation**, **Device test required**, **Open**, and **Blocked**.

## Gate A: Discord Rich Presence on Android

- [x] Official Discord documentation states that Social SDK 1.10+ supports Rich Presence through unauthenticated RPC on Android.
- [x] Official documentation lists these requirements: Discord Android installed and signed in, and a registered Discord application with a valid Application ID.
- [x] Official docs describe the basic flow: set Application ID, configure an activity, call `UpdateRichPresence`.
- [x] Official documentation confirms that SDK downloads are accessed through the Social SDK section of the Developer Portal.
- [ ] Confirm Discord authorizes a standalone companion app that detects a third-party game's foreground state. Do not misrepresent the app as a game to pass a form.
- [ ] Inspect the authorized SDK archive for Android libraries, supported ABIs, package size, license/redistribution requirements, and minimum Android/NDK/CMake requirements.
- [ ] Confirm the SDK can be integrated into a standalone Kotlin Android app rather than only an intended game-engine setup.
- [ ] Build a minimal proof of concept that publishes a test activity, updates it, and clears it from the signed-in Discord Android client.
- [ ] Verify failure handling when Discord is closed, signed out, or disconnected.
- [ ] Verify the intended activity fields and artwork behavior on an actual Discord profile.

**Gate result:** Android RPC is documented, but the companion-app use case, archive contents, and device behavior remain unverified.

## Gate B: Minecraft Bedrock foreground detection

- [ ] Confirm the installed Minecraft package can be identified reliably on the target device.
- [ ] Install a test app and verify CraftPresence can receive usage events after the owner grants CraftPresence its own Usage Access.
- [ ] Test whether the OS reports Minecraft foreground/background transitions consistently on Android 14 / API 34.
- [ ] Test behavior after screen lock, app switching, process recreation, battery optimization, and reboot.
- [ ] Confirm a 15-second app-switch grace period avoids false session endings without leaving presence stale.
- [ ] Verify manual Stop and manual disable always clear presence immediately.
- [ ] Document the exact permission explanation and recovery instructions if Usage Access is unavailable.

**Important:** The owner reports Minecraft appears to have Usage Access. That does not grant access to CraftPresence. Usage Access is an OS-level permission and may be exposed differently by device firmware. Do not attempt to bypass Android restrictions.

**Gate result:** Open. No successful CraftPresence device test is recorded yet.

## Gate C: Build on the Android tablet using Termux

Known environment notes from the project owner:
- Samsung Galaxy Tab S6 Lite, Android 14 / API 34, ARM64.
- Termux is installed; Java 21.0.13 is available.
- Gradle 9.3.1 has been downloaded and its version command succeeds.
- clang is installed; CMake and sdkmanager were not found at last check.
- Approximately 11 GB of free storage was reported at last check.
- ANDROID_HOME and ANDROID_SDK_ROOT are unset.

Checklist:
- [ ] Confirm a supported Android SDK installation route in Termux and estimate its disk usage.
- [ ] Confirm Android NDK and CMake requirements for the Discord native SDK, if required.
- [ ] Verify SDK/NDK/Gradle/Android Gradle Plugin version compatibility before downloading large packages.
- [ ] Build and install a minimal empty Android app on the tablet.
- [ ] Build a minimal JNI/native-library sample if the selected SDK requires native integration.
- [ ] Confirm a debug APK installs and launches on the target device.
- [ ] Record the exact commands, tool versions, disk usage, and any reproducible errors.

**Gate result:** Open. Gradle availability alone does not prove Android builds can complete.

## Phase 0 exit criteria

Phase 0 passes only when:
1. Discord integration route, supported package/ABI, and license/redistribution terms are understood.
2. A small proof of concept successfully publishes, updates, and clears presence on the target tablet, or a documented technical blocker forces an explicit redesign.
3. Foreground detection has been tested with real permission behavior on the target tablet.
4. The tablet can build and install a native Android test app, including native code if required.
5. Architecture decisions are updated to reflect measured results.

## Evidence log

| Date | Area | Evidence | Result |
|---|---|---|---|
| 2026-10-23 | Discord | Official Rich Presence guide, SDK installation/release notes, overview, and SDK Terms | Android unauthenticated RPC is documented; companion-app use and package details need confirmation |
| 2026-10-23 | Termux | User-provided command output | Java 21 and Gradle 9.3.1 work; clang present; SDK/NDK toolchain not configured; ~11 GB free |
| Pending | Detection | Target-device Usage Access test | Minecraft has Usage Access reported; CraftPresence test not done |
| Pending | Build | Minimal Android build/install from Termux | Not tested |
