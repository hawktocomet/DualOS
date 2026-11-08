# DualOS Architecture

## Overview

DualOS is planned as an Android-based operating system installed on removable microSD storage.

The intended boot arrangement is:

1. Device firmware starts the bootloader.
2. GBL provides the boot path for the removable-storage system.
3. OpenABL provides the operating-system selection interface.
4. Selecting DualOS boots the Android-based system from microSD.
5. Selecting stock Android boots the original internal installation.
6. With the microSD removed, the original stock Android installation remains the fallback.

The internal eMMC installation is not a target for DualOS installation or modification.

This is the target architecture. A complete Android boot from microSD through GBL has not yet been demonstrated.

## Platform layers

### 1. Boot and selection

Responsibilities:

- GBL-based boot support.
- Detection and use of the removable-storage boot device.
- OpenABL operating-system selection.
- Selection of the stock Android fallback.
- Safe behavior when the microSD card is absent or unusable.

The bootloader must not depend on modifying the stock Android installation to provide the alternative boot path.

### 2. Android system

The DualOS system image and its required supporting partitions or files will reside on microSD.

The implementation must establish which Android boot images, partition layout, device configuration, and vendor components are required for the Duo Lite.

The exact storage layout and Android image construction method remain engineering decisions until validated.

### 3. Hardware integration

The platform must support:

- Both displays and their independent characteristics.
- Touch input on both displays.
- Physical controls and button mapping.
- GPU acceleration.
- Audio.
- Wi-Fi and Bluetooth.
- Power management, battery reporting, and charging.
- Thermal monitoring and fan control where supported.
- Suspend, resume, and shutdown behavior.

Existing Android and Linux observations should guide testing, but behavior must be verified on the actual DualOS Android build.

### 4. DualOS system services

Planned services include:

- Display coordination and screen-role management.
- Physical-control routing.
- Game library and metadata management.
- Performance profiles and hardware-aware configuration.
- Shader and pipeline cache management.
- Local multiplayer discovery and connectivity.
- Streaming integration.

### 5. First-party home environment

The DualOS home experience is a first-party interface designed around the Duo Lite.

Top-level sections:

- Play
- Watch
- Stream
- Settings

Companion pages:

- Game Spotlight
- Friends
- Multiplayer

The interface should remain curated and consistent rather than exposing a general-purpose launcher replacement system.

### 6. Game execution

The Play experience should integrate:

- Android games.
- Supported emulators.
- Supported native-recompiled titles.
- Streamed PC games.

For compatible 3DS titles, optional static recompilation may provide an alternative to emulation. Emulator compatibility remains necessary as a fallback.

## Storage and preservation requirements

- DualOS system files reside on microSD.
- The original stock Android installation remains on internal eMMC.
- DualOS development must not require flashing or repartitioning internal eMMC.
- Removing the card must preserve the ability to boot stock Android.
- Recovery and rollback procedures must be documented before distributing system images.

## Engineering status and validation boundaries

Demonstrated:

- ROCKNIX boot from microSD.
- GBL EFI application boot from microSD.
- GBL diagnostic access to the loaded-image device's filesystem.
- Successful diagnostic report write to the microSD filesystem.

Not yet demonstrated:

- Complete Android system boot from microSD through GBL.
- OpenABL selection and handoff for the finished DualOS installation.
- Full hardware support under the proposed Android build.
- The integrated DualOS home environment.

## Build and validation principles

- Keep bootloader work separate from Android system work.
- Preserve reproducible source revisions and build instructions.
- Validate each boot-chain transition independently.
- Keep internal eMMC untouched.
- Record successful device tests separately from assumptions and planned behavior.
