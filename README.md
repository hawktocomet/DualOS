# DualOS

**An Android-based, dual-screen gaming operating system for the Retroid Pocket Duo Lite.**

DualOS is designed to boot from a removable microSD card through our Generic Bootloader (GBL), while preserving the device's original stock Android installation on internal storage.

The goal is to give the Duo Lite a purpose-built gaming experience without sacrificing its original operating system.

## Core concept

- **DualOS lives on microSD.** Its Android-based system installation and supporting data reside on removable storage.
- **Stock Android stays untouched.** The internal eMMC installation remains the recovery and everyday fallback.
- **GBL handles boot.** Our GBL-based bootloader provides the path to boot the microSD-based Android system.
- **OpenABL selects the system.** Users can choose which operating system to boot through OpenABL.
- **Android is the foundation.** DualOS builds on Android's application compatibility, drivers, and hardware support rather than creating a separate Linux distribution.

## Product priorities

1. A purpose-built dual-screen gaming interface.
2. Excellent Nintendo 3DS emulation, with reliable 2x rendering as a key performance goal.
3. A unified library for installed games and streamed PC games.
4. Local multiplayer between Duo Lite devices.
5. Optional on-device native recompilation where supported, with emulation retained as a fallback.
6. Offline shader and pipeline cache management to reduce stutter.
7. A consistent, curated experience with minimal user configuration.

## Interface direction

The planned first-party home environment uses four top-level sections:

- Play
- Watch
- Stream
- Settings

The Play experience will include the game library and integrated streaming. Additional companion pages are planned for Game Spotlight, Friends, and Multiplayer.

## Current engineering status

Completed or demonstrated:

- ROCKNIX has successfully booted from microSD on the Duo Lite.
- A custom GBL EFI diagnostic application has booted from microSD.
- The diagnostic application successfully identified its loaded-image device, accessed that device's filesystem, and wrote a report to the card.
- The original stock Android installation remains on internal storage.

Not yet demonstrated:

- Booting a complete Android-based DualOS installation from microSD through GBL.
- OpenABL operating-system selection for the completed DualOS installation.
- The complete DualOS Android system and first-party interface.

Linux and ROCKNIX work remains useful as hardware research and boot-chain validation. It is no longer the planned operating-system foundation.

Detailed engineering evidence and historical investigation reports are maintained in the private [duo-lite-reports](https://github.com/hawktocomet/duo-lite-reports) repository.

## Initial target

Retroid Pocket Duo Lite. Other dual-screen handhelds, including the AYN Thor, may be considered later.

## Project principles

- Preserve the original internal Android installation.
- Keep DualOS removable and independently maintainable.
- Prefer proven hardware support over unnecessary reinvention.
- Make dual-screen behavior central to the experience.
- Prioritize measurable gaming performance and reliability.
- Keep the system reproducible and maintainable.

## Documentation

- [Vision](docs/VISION.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Roadmap](docs/ROADMAP.md)
- [Hardware notes](docs/HARDWARE.md)
- [GBL bootloader development](docs/BOOTLOADER-GBL.md)
