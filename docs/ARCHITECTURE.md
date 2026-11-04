# DualOS Architecture

## Proposed layers

### Boot and platform
Responsible for boot flow, kernel, device tree, initramfs, root filesystem, storage discovery, and recovery.

### Hardware services
Responsible for display discovery, GPU and CPU frequency interfaces, input devices, audio, battery, thermal management, wireless networking, and power states.

### Display system
Responsible for both physical displays, output orientation, refresh behavior, composition, window placement, and application display assignment.

### DualOS shell
Responsible for the top-screen home experience, navigation, library, settings, and system overlays.

### Game services
Responsible for game metadata, launch configuration, emulator integration, save data, controller mapping, and per-game settings.

### Streaming
Responsible for PC discovery, pairing, streaming sessions, and integration with the unified game library.

### Applications
Emulators, game engines, media applications, and other user-facing software.

## Design constraints

- Keep hardware-specific paths out of the shell.
- Discover capabilities at runtime where possible.
- Keep thermal protection under kernel/platform control.
- Avoid assuming both displays share identical timing behavior.
- Treat streaming as a game-launch backend.
- Keep user data separate from replaceable system files.

## Initial implementation strategy

Establish a bootable and debuggable Linux foundation before building the full shell. Validate hardware services independently, then integrate them into the user experience.
