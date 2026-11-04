# DualOS Vision

## Purpose

DualOS is a cohesive gaming platform for dual-screen handhelds. It should eliminate the need to assemble a usable experience from unrelated launchers, emulators, streaming clients, configuration utilities, and device-specific workarounds.

## The dual-screen experience

The two displays have complementary roles:

- **Top screen:** home, navigation, library, discovery, settings, and contextual information.
- **Bottom screen:** active game or content surface.

The interface should make use of both displays without requiring every application to do so. Single-screen applications must remain usable.

## One Play experience

Local games and streamed PC games belong in the same library.

Streaming is an implementation detail of launching a game, not a separate top-level destination. A PC game should be discoverable and launchable through the same flow as a locally installed game.

Moonlight or an equivalent streaming client should operate underneath the DualOS shell.

## Product principles

1. Dual-screen behavior is a platform feature, not an afterthought.
2. The default experience is curated and straightforward.
3. Performance, compatibility, and battery life are balanced deliberately.
4. Device-specific functionality belongs behind hardware abstractions.
5. The system should be maintainable by a small development effort.
6. The original device OS should remain recoverable.

## Initial target

Retroid Pocket Duo Lite. The architecture should avoid unnecessarily coupling the platform to this one device.
