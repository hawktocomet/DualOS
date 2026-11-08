# DualOS Roadmap

This roadmap reflects the current decision to build DualOS on Android, booted from microSD through GBL, with OpenABL providing operating-system selection.

Items are goals until verified on the device.

## Phase 1 — Boot-chain foundation

- [x] Boot ROCKNIX from microSD on the Duo Lite.
- [x] Build and boot a custom GBL EFI diagnostic application from microSD.
- [x] Verify GBL can identify the loaded-image device and access its filesystem.
- [x] Write and read back a diagnostic report on the microSD card.
- [ ] Establish the complete Android boot requirements for the Duo Lite.
- [ ] Determine the required microSD Android storage and partition layout.
- [ ] Boot a minimal Android-based system from microSD through GBL.
- [ ] Validate the OpenABL selection and handoff flow.
- [ ] Verify stock Android fallback with the microSD removed.
- [ ] Document recovery and rollback procedures.

## Phase 2 — Android platform

- [ ] Establish a reproducible Android build or porting workflow.
- [ ] Identify required device-specific kernel, vendor, and hardware components.
- [ ] Validate both displays, touch input, audio, GPU, and physical controls.
- [ ] Validate Wi-Fi, Bluetooth, battery reporting, charging, and power management.
- [ ] Validate suspend, resume, reboot, and shutdown.
- [ ] Confirm the internal stock Android installation remains untouched.

## Phase 3 — Dual-screen system behavior

- [ ] Implement reliable display coordination.
- [ ] Establish screen-role and screen-swap behavior.
- [ ] Implement physical-control focus and companion-menu behavior.
- [ ] Validate touch and physical input across both displays.
- [ ] Measure frame pacing and synchronization under simultaneous display activity.

## Phase 4 — 3DS-first gaming

- [ ] Evaluate Android 3DS emulator options on the Duo Lite.
- [ ] Establish a repeatable performance and compatibility test suite.
- [ ] Prioritize reliable 2x rendering where feasible.
- [ ] Investigate shader and pipeline compilation stutter.
- [ ] Implement offline cache management.
- [ ] Evaluate optional native recompilation for supported games.
- [ ] Preserve emulator fallback for every applicable title.

## Phase 5 — Game library and streaming

- [ ] Build the unified Play library.
- [ ] Integrate local Android games and supported emulators.
- [ ] Integrate supported native-recompiled games.
- [ ] Integrate PC streaming through Moonlight or an equivalent solution.
- [ ] Add game artwork and metadata management.

## Phase 6 — Multiplayer and social features

- [ ] Establish local multiplayer between two Duo Lite devices.
- [ ] Build device discovery and connection management.
- [ ] Design the Friends experience.
- [ ] Investigate future remote play and Nano Link services.

## Phase 7 — First-party DualOS experience

- [ ] Implement the fixed DualOS home environment.
- [ ] Implement Play, Watch, Stream, and Settings.
- [ ] Implement Game Spotlight, Friends, and Multiplayer companion pages.
- [ ] Add curated wallpaper, artwork, music, and interface sounds.
- [ ] Validate the complete experience with physical controls and touch.

## Phase 8 — Stability and expansion

- [ ] Establish system update and rollback procedures.
- [ ] Establish compatibility and regression testing.
- [ ] Publish reproducible build and installation documentation.
- [ ] Evaluate support for additional devices, including the AYN Thor.
