# DualOS Vision

## Purpose

DualOS is an Android-based gaming operating system designed around the Retroid Pocket Duo Lite's dual-screen hardware.

It is intended to provide a cohesive, purpose-built gaming experience while keeping the device's original stock Android installation intact.

DualOS will reside on microSD and boot through our GBL bootloader. OpenABL will provide the operating-system selection interface. Removing the microSD card will leave the original internal Android installation available as the fallback.

## Why Android

The project initially explored a Linux-based system using ROCKNIX and related work. That investigation established useful boot and hardware knowledge, but Android is now the chosen foundation.

Android offers a more direct path to application compatibility and existing device support. It also provides a foundation for integrating emulators, native recompilation tools, streaming clients, and the broader Android gaming ecosystem.

The objective is not simply to install another Android build. It is to create a distinct operating-system experience tailored to this hardware.

## Primary goals

### 1. Dual-screen-first experience

The two displays should work together as a single gaming environment. Screen roles, focus, touch, physical controls, and display synchronization must be treated as core platform behavior.

### 2. 3DS-first gaming

Nintendo 3DS is the initial performance priority. The goal is reliable 2x rendering wherever the hardware and software permit it, with smooth frame pacing and reduced shader-compilation stutter.

### 3. Unified game experience

The Play library should bring together locally installed games, supported native-recompiled titles, emulated games, and streamed PC games.

Streaming should be integrated into the Play experience rather than treated as an unrelated launcher.

### 4. Minimal configuration

DualOS should make sensible hardware-aware decisions automatically. Users should not need to understand emulator internals, graphics settings, or complicated performance tuning to get a good experience.

### 5. Local multiplayer and connectivity

Local multiplayer between two Duo Lite devices is an initial long-term goal. A broader friend and remote-play system may follow.

## Home experience

The planned first-party home environment is a fixed DualOS interface, not a replaceable collection of third-party front ends.

Top-level navigation:

- Play
- Watch
- Stream
- Settings

Additional swipe-accessible companion pages:

- Game Spotlight
- Friends
- Multiplayer

Customization should remain curated: wallpapers, game artwork, interface sounds, and music.

## Controls

Planned physical-control behavior:

- Home tap: return to the DualOS home experience.
- Back tap: open the companion menu and move control focus to it.
- Back hold: swap the physical display arrangement.
- Touch input remains available while the companion interface has physical-control focus.

These mappings require device-level validation before implementation is considered complete.

## Game technology

DualOS should support multiple ways to run games:

- Android emulation.
- Native Android games and applications.
- Optional on-device static recompilation for supported titles.
- PC game streaming.

Native recompilation must not eliminate the emulator fallback. Compatibility and user choice are more important than forcing every title through one execution method.

An offline shader and pipeline cache manager is an early priority. Network-assisted cache sharing may be considered later.

## Platform strategy

Android is the operating-system foundation. GBL and OpenABL provide the boot and selection path. The microSD installation must remain independent of the stock Android system on internal storage.

Linux and ROCKNIX are retained as sources of hardware research and validated boot-chain experience, not as the intended DualOS operating system.

## Scope

The Retroid Pocket Duo Lite is the first target. Expansion to other dual-screen handhelds may be considered after the Duo Lite platform is stable.
