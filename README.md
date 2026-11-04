# DualOS

**A unified gaming operating system designed for dual-screen handhelds.**

DualOS aims to make dual-screen handheld gaming feel intentional rather than fragmented. It brings local emulation, native Android/Linux applications where appropriate, and PC game streaming into one cohesive gaming environment.

## Product principles

- Dual-screen-first interface and interaction model.
- A curated, integrated gaming library.
- Local and streamed games presented together.
- Hardware-aware performance and power management.
- Reproducible builds and transparent device support.
- Installation that preserves the device's original operating system.

## Initial hardware target

Retroid Pocket Duo Lite (Qualcomm SM6125).

## Project status

Early platform research and architecture definition. No alternative OS boot has yet been demonstrated on the target hardware.

## Documentation

- [Vision](docs/VISION.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Hardware](docs/HARDWARE.md)
- [Roadmap](docs/ROADMAP.md)

## Safety and installation goals

The initial target is an alternative OS booted from removable storage while preserving stock Android on internal storage. Removing the removable storage must leave the device able to boot its original operating system.

No installation method will be described as supported until it has been demonstrated on the actual hardware.
