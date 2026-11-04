# DualOS Roadmap

## Phase 1 — Platform feasibility
- [x] Obtain and inspect a ROCKNIX SM6115 image.
- [x] Identify the Duo Lite device tree.
- [x] Identify Adreno 610 GPU devfreq configuration.
- [x] Trace ROCKNIX GPU profile loading and helper behavior.
- [ ] Establish a verified boot path from removable storage.
- [ ] Preserve stock Android boot when removable storage is absent.

## Phase 2 — Minimal bootable system
- Establish kernel and device-tree compatibility.
- Bring up storage, display, input, audio, and networking.
- Confirm both displays are independently usable.
- Establish logs, recovery, and repeatable image builds.

## Phase 3 — Hardware services
- Display management and dual-screen composition.
- GPU/CPU frequency and thermal policy.
- Battery and power management.
- Controller and input abstraction.
- Audio and wireless services.

## Phase 4 — Gaming platform
- Unified game library and launcher.
- Emulator integration.
- Save data and controller configuration.
- Streaming integrated into the Play experience.

## Phase 5 — Product experience
- Dual-screen home interface.
- Settings and per-game profiles.
- Installation and update workflow.
- Documentation and supported-device process.

## Current status

The project is in platform feasibility. A successful alternative-OS boot on the Retroid Pocket Duo Lite has not yet been demonstrated.
