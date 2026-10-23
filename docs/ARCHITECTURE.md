# CraftPresence Architecture Proposal

This is a proposal, not a statement that the integrations have been proven. Phase 0 results take precedence over this document.

## Product principles

- Android-first and tablet-friendly.
- Explicit permission consent, clear status, and useful diagnostics.
- Do not claim to know more about Minecraft than the available integration actually reveals.
- Local-first session history with user-controlled deletion.
- Low background activity and sensible battery use.
- No secrets in source control.
- Keep Discord integration behind an adapter so a blocked SDK can be replaced without rewriting UI and session logic.

## Proposed technology

- Kotlin for Android application logic.
- Jetpack Compose and Material 3 for the interface, if the on-device build toolchain supports them.
- Coroutines and StateFlow for asynchronous state.
- DataStore for lightweight settings.
- Room/SQLite for local session history, if the persistence dependency is practical in the build environment.
- Android UsageStatsManager for foreground-app observation, subject to permission and device testing.
- Discord Social SDK C++ integration through JNI only if the official SDK package and Android use case support it.
- Glance or RemoteViews for a later home-screen widget, subject to feasibility.

Do not add dependencies or download large toolchains until Phase 0 confirms the requirements.

## Proposed package layout

    app/src/main/java/.../
      ui/home/
      ui/profiles/
      ui/history/
      ui/settings/
      ui/components/
      detection/
      presence/
      session/
      automation/
      data/
      widget/
      diagnostics/
      integrations/

    app/src/main/cpp/discord/
    app/src/test/
    app/src/androidTest/
    docs/
    addon/behavior_pack/
    server-bridge/

The add-on and server-bridge folders are research-only future options, not part of the initial implementation.

## Core components

### Foreground detector

Observes app usage only after the user grants Android's required Usage Access permission. It reports observations to the session coordinator and does not directly manipulate the UI or Discord client.

### Session coordinator

Owns the session state machine and applies the 15-second app-switch grace period. Proposed states:

- IDLE: no Minecraft session detected.
- DETECTED: Minecraft foreground activity observed.
- ACTIVE: session is active and presence should be published.
- GRACE: app switch or temporary interruption is being evaluated.
- SUSPENDED: monitoring is unavailable or manually paused.
- ERROR: a recoverable integration failure is visible to the user.

Manual Stop overrides automation. Manual disable clears presence immediately. If Discord disconnects, retain local session state and use bounded reconnect attempts rather than repeatedly waking the device.

### Discord presence adapter

Provides a small interface for publish, update, clear, and connection diagnostics. It should hide native SDK details from the rest of the app. Do not implement it until SDK packaging, supported Android usage, and terms are checked.

### Profile and activity model

Proposed activity fields:
- Application ID.
- Details and state text.
- Large/small image keys and hover text.
- Start timestamp.
- Optional buttons only if the official integration supports the intended use.
- Selected profile and source metadata.

Validate text lengths and supported field combinations before sending updates. Never hardcode credentials or private identifiers into the app.

### Local session history

History is enabled by default per product decision. Store only information needed for the session timeline. Provide clear deletion controls and explain what is retained. Avoid collecting message content, account credentials, or unrelated app usage.

## UX direction

Theme: Ender-inspired, with a dark violet base and soft mint accents. Proposed palette:
- Base: #141020
- Surface: #29213e
- Primary: #8066bd
- Accent: #bda4f6
- Text: #d9f6ef

Design goals: subtle glass-like surfaces, readable contrast, restrained motion, reduced-motion support, accessible text sizing, and low battery overhead.

Planned screens:
- Home/live dashboard.
- Setup and permission guidance.
- Presence editor and live preview.
- Profiles.
- Session history.
- Settings.
- Automation rules.
- Artwork manager.
- Diagnostics and Integration Lab.
- About and privacy details.

## Privacy and reliability rules

- Show the monitoring state and whether required permission is missing.
- Explain why Usage Access is requested before opening system settings.
- Do not disguise or bypass Android permission controls.
- Never infer world, dimension, server, or game mode from foreground detection alone.
- Clear presence on manual Stop or manual disable.
- Keep reconnect attempts bounded and avoid tight polling loops.
- Make history deletion available in the app.
- Keep test logs free of tokens and sensitive data.

## Future integrations

An optional Minecraft add-on or server-side bridge may be investigated later. A behavior pack alone must not be assumed to communicate with an external app or Discord. Any server integration must be optional, documented, and tested separately from the core foreground-detection feature.
