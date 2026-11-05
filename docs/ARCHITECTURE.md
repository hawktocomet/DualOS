# DualOS Architecture

This document describes the proposed architecture. It is a target design, not a claim that all components are implemented.

## Proposed layers

### Boot, storage, and platform

Responsible for boot flow, kernel, device tree, initramfs, SD-rooted root filesystem, storage discovery, updates, and recovery.

Requirements:
- Keep the stock Android installation on internal storage untouched.
- Boot the alternative system from removable storage.
- Preserve the stock Android boot path when the removable card is absent.
- Keep user data separate from replaceable system files.
- Provide repeatable image builds, logs, and recovery procedures.

ROCKNIX ABL and ROCKNIX microSD boot have been demonstrated on the Duo Lite. A complete DualOS deployment and update/recovery design remain to be implemented and validated.

### Hardware services

Responsible for discovering and controlling:
- Both displays, touch panels, and display timing.
- GPU and CPU frequency interfaces.
- Thermal sensors, cooling devices, and fan policy.
- Buttons, analog controls, and other input devices.
- Audio input/output.
- Battery, charging, suspend/resume, and power states.
- Wi-Fi and Bluetooth.

Hardware-specific behavior should be encapsulated behind services or interfaces. Discover capabilities at runtime where practical rather than assuming fixed paths or values.

Thermal protection must remain under kernel/platform control. Userspace performance profiles must never disable or bypass thermal safeguards.

### Display system

Responsible for both physical displays, output orientation, refresh behavior, composition, window placement, application display assignment, and focus.

Do not assume both displays share identical timing behavior. Validate composition, synchronization, touch mapping, frame pacing, and application compatibility on the actual hardware.

### DualOS shell

Responsible for:
- Top-level Play, Watch, Stream, and Settings navigation.
- Home experience and library.
- Lower-screen companion pages: Game Spotlight, Friends, and Multiplayer.
- System overlays and companion menu.
- Physical-control focus and screen-swap behavior.

The shell is a fixed first-party experience, not a generic desktop or launcher marketplace.

### Game services

Responsible for:
- Unified game metadata and library.
- Game launch orchestration.
- Emulator integration and per-game profiles.
- Save data and controller mapping.
- Per-title compatibility and performance information.
- Local and streamed game entries in one Play experience.

### Nano 3DS environment

A curated 3DS-focused subsystem is the first major gaming priority.

Responsibilities:
- Emulator integration and validated per-game configuration.
- Local shader and pipeline cache storage, inspection, and management.
- Reliable cache persistence and warm-start behavior.
- Optional per-title static recompilation builds where validated.
- Emulation fallback for titles without a suitable recompilation path.

A network shader-cache service is explicitly deferred. Core cache functionality should work offline.

Static recompilation must be treated as an optional per-title backend. It requires correctness and performance validation and must not be assumed to work for arbitrary games.

### Multiplayer services

First target: local Wi-Fi discovery and multiplayer between two nearby Duo Lite devices with minimal setup.

Later capabilities may include friends, presence, invitations, remote rendezvous, and additional supported devices. Cloud services must not be prerequisites for local multiplayer.

### Streaming

Responsible for PC discovery, pairing, streaming sessions, and integration with the unified game library.

Moonlight or an equivalent client should operate as a backend. A streamed PC game should appear in the same Play experience as a local game.

### Applications

Emulators, game engines, media applications, streaming clients, and other user-facing software.

Applications should integrate with the platform's display, input, audio, library, and lifecycle services where appropriate.

## Design constraints

- Keep hardware-specific paths out of the shell.
- Discover capabilities at runtime where possible.
- Keep thermal protection under kernel/platform control.
- Avoid assuming both displays share identical timing behavior.
- Treat streaming as a game-launch backend.
- Keep user data separate from replaceable system files.
- Prefer local/offline functionality for core gaming features.
- Preserve normal emulation as fallback for optional recompilation.
- Preserve stock Android on internal storage.

## Initial implementation strategy

1. Audit DuoLinux and ROCKNIX source trees and identify reusable platform components, patches, build tooling, and Duo Lite-specific support.
2. Define the SD-rooted deployment, update, and recovery model.
3. Validate hardware services independently: displays, input, audio, GPU, networking, thermals, and power.
4. Establish repeatable 3DS performance and compatibility baselines against stock Android.
5. Prototype the smallest set of Linux/Nano changes likely to produce measurable improvements.
6. Integrate game services and the fixed dual-screen shell after the platform foundation is understood.

Do not build the full shell on top of unverified hardware assumptions.
