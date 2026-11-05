# DualOS

**A unified, dual-screen gaming operating system for handhelds.**

DualOS is a Duo Lite-first project to create a polished, purpose-built gaming environment that makes dual-screen handheld gaming feel intentional rather than fragmented. The initial engineering priority is an excellent Nintendo 3DS experience, followed by a unified local and streamed game library, multiplayer, and a complete dual-screen interface.

## Product principles

- Dual-screen-first interface and interaction model.
- Preserve the device's original operating system on internal storage.
- Self-contained, reversible installation rooted on removable storage.
- Curated gaming experience rather than a collection of unrelated launchers and configuration tools.
- Local games and streamed PC games presented through one Play experience.
- Hardware-aware performance, thermal, and power management.
- Reproducible builds, evidence-based device support, and recoverability.

## Initial hardware target

Retroid Pocket Duo Lite (Qualcomm SM6125 / Adreno 610).

The architecture should avoid unnecessary coupling to this device so that other dual-screen handhelds, potentially including the AYN Thor, can be evaluated later.

## Current project status

- ROCKNIX ABL is installed on the Duo Lite.
- ROCKNIX has successfully booted from microSD on the target hardware.
- The project has moved beyond basic removable-media boot feasibility into platform integration and product engineering.
- A complete DualOS system, polished shell, and supported installation/update process have not yet been delivered.
- The next major engineering task is a source-level audit of DuoLinux and ROCKNIX to map reusable components, required changes, and Duo Lite-specific gaps.

## Initial product priorities

1. Establish a reliable SD-rooted Linux platform while preserving stock Android on internal storage.
2. Audit and integrate Duo Lite hardware support: both displays, controls, touch, audio, GPU, Wi-Fi, Bluetooth, thermals, and power.
3. Benchmark 3DS performance against the best available stock Android experience before committing to a custom gaming stack.
4. Build a curated 3DS environment with validated per-game configuration and reliable shader/pipeline cache handling.
5. Evaluate optional per-title static recompilation with normal emulation retained as fallback.
6. Add local multiplayer between nearby DualOS devices.
7. Deliver the unified Play experience, streaming integration, and dual-screen shell.

## Documentation

- [Vision](docs/VISION.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Hardware](docs/HARDWARE.md)
- [Roadmap](docs/ROADMAP.md)

Detailed engineering evidence and historical investigation reports are maintained in the private [duo-lite-reports](https://github.com/hawktocomet/duo-lite-reports) repository.

## Safety and installation goals

The product target is an alternative operating system rooted on removable storage, with the stock Android installation on internal storage left intact. Removing the microSD card must leave the device able to boot its original operating system.

ROCKNIX has already demonstrated microSD boot on the Duo Lite using ROCKNIX ABL. That milestone does not, by itself, establish a complete DualOS installation or a supported consumer installation process.

No internal-storage modification, destructive installation, or supported-release claim is acceptable without explicit engineering justification, device-specific validation, and a recovery plan.
