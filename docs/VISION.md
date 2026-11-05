# DualOS Vision

## Purpose

DualOS is a cohesive gaming platform for dual-screen handhelds. It should eliminate the need to assemble a usable experience from unrelated launchers, emulators, streaming clients, configuration utilities, and device-specific workarounds.

The first target is the Retroid Pocket Duo Lite. The product should feel like a modern, evolved successor to the Nintendo 3DS: two displays with complementary roles, a curated game-first interface, frictionless local multiplayer, and a consistent experience across supported hardware.

## First product objective: excellent 3DS gaming

The first major gaming objective is to make Nintendo 3DS titles run exceptionally well on the Duo Lite.

The best current stock Android experience identified by the project is Citra MMJ. Linux is not automatically faster or better. Before investing in a custom emulator stack, DualOS must establish repeatable comparisons for frame pacing, shader compilation, compatibility, audio, thermals, and battery behavior.

The intended experience includes:
- Curated, validated per-game emulator profiles; users should not need to tune dozens of settings.
- Local shader and pipeline cache management that works offline.
- Reliable warm-start behavior and transparent handling of shader compilation.
- Optional per-title static recompilation where technically viable and validated.
- Ordinary emulation as a compatibility fallback whenever a recompilation build is unavailable or unsuitable.

Static recompilation is an optimization path, not a promise of universal native ports or broad title coverage.

## The dual-screen experience

The two displays have complementary roles:
- **Top screen:** home, navigation, library, settings, and contextual information.
- **Bottom screen:** active game or content surface, plus companion pages and controls when appropriate.

The interface should use both displays intentionally without requiring every application to do so. Single-screen applications must remain usable.

## Navigation and home experience

The top-level tabs are:
- **Play**
- **Watch**
- **Stream**
- **Settings**

The Play experience is the primary library for local games and streamed PC games. Stream should also be integrated into the Play library through Moonlight or an equivalent backend; streaming is a launch method, not a separate game ecosystem.

Initial lower-screen swipe pages:
- Game Spotlight
- Friends
- Multiplayer

Do not add a Discover page to the initial design.

Interaction concepts:
- Back tap opens the companion menu.
- Back hold swaps screens.
- Home returns to the home experience.
- The companion interface receives physical-control focus while touch remains available.

These input mappings are product design targets and must be validated against the device's actual input stack.

## One Play experience

Local games and streamed PC games belong in the same library.

A PC game should be discoverable and launchable through the same flow as a locally installed game. Moonlight or an equivalent streaming client should operate underneath the DualOS shell.

## Multiplayer

Local Wi-Fi multiplayer between two nearby Duo Lite devices is the first multiplayer target. Discovery and joining should require minimal setup.

Friend presence, invitations, remote rendezvous, and internet-based multiplayer coordination are later capabilities. Core local play should not depend on a cloud service.

## Curated system experience

DualOS should provide a fixed, first-party front end rather than alternate launcher or front-end choices. The visual system should include curated sounds, interface music, wallpapers, and icon art.

The intended home experience is modern, balanced, and recognizably designed for two screens. Interface music and sounds are enabled by default, with user controls in Settings.

## Product principles

1. Dual-screen behavior is a platform feature, not an afterthought.
2. The default experience is curated and straightforward.
3. Performance, compatibility, thermals, and battery life are balanced deliberately.
4. Device-specific functionality belongs behind hardware abstractions.
5. Local/offline operation is preferred for core features.
6. The system should be maintainable by a small development effort.
7. The original device OS should remain intact and recoverable.

## Initial target and future portability

Retroid Pocket Duo Lite is the first target. The architecture should avoid unnecessarily coupling the platform to this one device. Additional dual-screen handhelds, potentially including the AYN Thor, can be considered after the Duo Lite experience is established.
