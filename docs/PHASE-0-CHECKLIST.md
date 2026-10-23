# Phase 0: Feasibility Checklist

**Rule:** do not begin application implementation until the required gates below have evidence. A document or web page is research evidence, not proof that the feature works on the target tablet.

Status labels: **Verified in documentation**, **Device test required**, **Open**, and **Blocked**.

## Gate A: Discord Rich Presence on Android

- [x] Official Discord documentation states that Social SDK 1.10+ supports Rich Presence through unauthenticated RPC on Android.
- [x] Official documentation lists these requirements: Discord Android installed and signed in, and a registered Discord application with a valid Application ID.
- [ ] Confirm the current SDK download, supported Android package/ABI, minimum platform requirements, redistribution rules, and license terms.
- [ ] Confirm the SDK can be integrated into a standalone Kotlin Android app rather than only an intended game-engine setup.
- [ ] Build a minimal proof of concept that publishes a test activity, updates it, and clears it from the signed-in Discord Android client.
- [ ] Verify failure handling when Discord is closed, signed out, or disconnected.
- [ ] Verify the intended activity fields and artwork behavior on an actual Discord profile.

**Gate result:** Documentation feasibility is promising; implementation feasibility remains unproven until the SDK packaging and device test pass.

## Gate B: Minecraft Bedrock foreground detection

- [ ] Confirm the installed Minecraft package can be identified reliably on the target device.
- [ ] Find and test the device's Usage Access settings and determine whether CraftPresence can receive usage events after explicit user authorization.
- [ ] Test whether the OS reports Minecraft foreground/background transitions consistently on Android 14 / API 34.
- [ ] Test behavior after screen lock, app switching, process recreation, battery optimization, and reboot.
- [ ] Confirm a 15-second app-switch grace period avoids false session endings without leaving presence stale.
- [ ] Verify manual Stop and manual disable always clear presence immediately.
- [ ] Document the exact permission explanation and recovery instructions if Usage Access is unavailable.

**Important:** Usage Access is an OS-level permission and may be exposed differently by device firmware. Do not assume that a permission screen or API behavior exists until it is observed on the target device. Do not attempt to bypass Android restrictions.

**Gate result:** Open. No successful device test is recorded yet.

## Gate C: Build on the Android tablet using Termux

Known environment notes from the project owner:
- Samsung Galaxy Tab S6 Lite, Android 14 / API 34, ARM64.
- Termux is installed; Java 21.0.13 is available.
- Gradle 9.3.1 has been downloaded and its version command succeeds.
- clang is installed; CMake was not installed at last check.
- Approximately 11 GB of free storage was reported at last check.
- Android SDK/NDK paths were not found during the earlier check.

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
| 2026-10-23 | Discord | Official Discord Social SDK documentation and release notes | Android unauthenticated RPC Rich Presence is documented for SDK 1.10+; device test pending |
| Pending | Detection | Target-device Usage Access test | Not tested |
| Pending | Build | Minimal Android build/install from Termux | Not tested |
