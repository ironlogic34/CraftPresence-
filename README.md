# CraftPresence

CraftPresence is a planned Android companion app that detects when Minecraft Bedrock is in the foreground and publishes a configurable activity to the signed-in Discord Android client.

**Project status: Phase 0 feasibility research. No application code has been started.** The core risks must be validated before choosing the final implementation.

## Goals

- Detect Minecraft Bedrock foreground sessions on the user's own Android device, with permission.
- Publish, update, and clear Discord Rich Presence through a supported integration.
- Provide an Ender-inspired interface, custom activity profiles, a session timer, and local session history.
- Remain battery-conscious, transparent about permissions, and usable on an Android tablet without a PC.
- Keep session history on-device and provide clear controls to delete it.

## Non-goals and boundaries

- CraftPresence does not modify the Minecraft app or claim to read private game internals.
- Foreground detection alone cannot reliably identify a world, server, dimension, or game mode. Those details must not be shown unless a separately verified integration supplies them.
- No credentials, Discord tokens, signing keys, or private user data belong in source control.
- No app implementation begins until the Phase 0 checklist records evidence for the main feasibility questions.

## Current target

- Minimum Android version: Android 10 / API 29.
- Initial test device: Samsung Galaxy Tab S6 Lite running Android 14 / API 34.
- Build environment: Android device using Termux; no PC or laptop assumed.
- Initial repository: public by project-owner choice. Never commit secrets or sensitive logs.

## Project documents

- [Phase 0 checklist](docs/PHASE-0-CHECKLIST.md)
- [Architecture proposal](docs/ARCHITECTURE.md)
- [Research notes and sources](docs/RESEARCH.md)

## Planned milestones

1. Phase 0: verify Discord SDK/RPC requirements, Minecraft detection permissions, and on-device build feasibility.
2. v0.1: minimal manual Rich Presence proof of concept.
3. v0.2: foreground detection and session state.
4. v0.3: main UI, profiles, and local history.
5. v0.4: automation, widget, and carefully scoped experimental integrations.
6. v0.9: battery, reconnect, privacy, and regression testing.
7. v1.0: stable release after device testing.

Milestones are targets, not promises. Any unsupported or blocked integration must be documented honestly rather than simulated as working.
