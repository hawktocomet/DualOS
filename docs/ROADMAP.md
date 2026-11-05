# DualOS Roadmap

This roadmap distinguishes completed feasibility milestones from work still required to deliver a complete DualOS product.

## Phase 1 — Platform feasibility and source audit

- [x] Obtain and inspect a ROCKNIX SM6115 image.
- [x] Identify the Duo Lite device tree.
- [x] Identify Adreno 610 GPU devfreq configuration.
- [x] Trace ROCKNIX GPU profile loading and helper behavior.
- [x] Install ROCKNIX ABL on the Duo Lite.
- [x] Demonstrate ROCKNIX booting from microSD on the actual Duo Lite.
- [ ] Audit DuoLinux and ROCKNIX source trees and identify reusable components, patches, build tooling, and Duo Lite-specific changes.
- [ ] Define the complete DualOS SD-rooted deployment and recovery architecture.
- [ ] Reconfirm and document stock Android boot behavior with the removable card absent as part of a formal acceptance test.

## Phase 2 — Minimal, maintainable SD-rooted platform

- Establish a reproducible kernel, device-tree, initramfs, and root-filesystem build.
- Bring up storage, both displays, touch, physical controls, audio, and networking.
- Confirm stock Android remains intact and boots when the microSD card is absent.
- Separate persistent user data from replaceable system files.
- Establish logs, recovery, update/rollback, and repeatable image builds.
- Avoid internal eMMC modification as a product requirement.

## Phase 3 — Hardware services and performance foundation

- Display management and dual-screen composition.
- GPU/CPU frequency discovery and conservative performance policy.
- Thermal and fan integration that preserves kernel safeguards.
- Controller and input abstraction.
- Audio, battery, charging, suspend/resume, and power management.
- Wi-Fi and Bluetooth services.
- Measure frame pacing and synchronization independently on both displays.

## Phase 4 — 3DS-first Nano gaming environment

- Establish repeatable stock Android Citra MMJ performance and compatibility baselines.
- Evaluate Linux emulator options and graphics-driver behavior on the Duo Lite.
- Compare frame times, shader compilation, cache persistence, audio, thermals, and battery behavior.
- Implement curated per-game configuration with minimal user tuning.
- Implement local/offline shader and pipeline cache management.
- Evaluate per-title static recompilation (including 3dsrecomp and Zakuro research) with correctness and performance validation.
- Preserve normal emulation as fallback.
- Do not claim a performance advantage until repeatable measurements demonstrate it.

## Phase 5 — Gaming platform and multiplayer

- Unified local-game library and launch orchestration.
- Game metadata, save data, and controller profiles.
- Local Wi-Fi discovery and multiplayer between two Duo Lite devices.
- Minimal-setup joining and session handling.
- Later: friends, presence, invitations, remote rendezvous, and additional devices.

## Phase 6 — Product experience

- Fixed first-party dual-screen shell.
- Top-level Play, Watch, Stream, and Settings navigation.
- Lower-screen Game Spotlight, Friends, and Multiplayer pages.
- Stream integration through Moonlight or equivalent in the unified Play experience.
- Companion menu, screen-swap, and physical-control focus behavior.
- Curated interface sounds, music, wallpapers, and icon art.
- Settings, per-game profiles, installation/update workflow, and user documentation.

## Phase 7 — Additional device support

- Evaluate other dual-screen handhelds only after the Duo Lite experience is stable.
- Keep device-specific support behind hardware abstractions.
- AYN Thor is a possible next target, not a current release commitment.

## Current status

DualOS has passed the basic alternative-OS removable-storage boot milestone: ROCKNIX ABL is installed and ROCKNIX has successfully booted from microSD on the Retroid Pocket Duo Lite.

The project is now in source audit, platform integration, and product feasibility. The next engineering milestone is the DuoLinux/ROCKNIX source-level audit, followed by a measured 3DS performance comparison against the best stock Android baseline.

A complete DualOS distribution, polished shell, supported installer, update system, and recovery process do not yet exist.
