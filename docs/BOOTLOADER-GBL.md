# GBL Bootloader Development

## Goal

DualOS is planned as an Android-based operating system booted from microSD through Generic Bootloader (GBL). The original stock Android installation on internal eMMC must remain untouched.

Intended behavior:
- microSD inserted: boot DualOS Android from the card.
- microSD removed: fall back to stock Android on internal storage.
- OpenABL: provide operating-system selection.

## Source and build

- Upstream: https://android.googlesource.com/platform/bootable/libbootloader
- Local workspace: ~/source/DualOS-tools/aosp-uefi-gbl
- Source checkout: bootable/libbootloader
- Revision: 2392920e8324
- Branch metadata: gbl-mainline
- Build target: @@gbl+//efi:aarch64_dev
- Build log: ~/gbl-aarch64-dev-build.log

Build command:
```bash
./bootable/libbootloader/gbl/bazel.sh build --extra_toolchains=@gbl//toolchain
```

Build completed successfully: 894 actions, 239.745 seconds elapsed. Toolchain: Bazel 9.2.0, Rustc 22.1.2, Clang 22.0.2.

## Diagnostic implementation

Modified files:
- gbl/efi/src/efi_blocks.rs: diagnostic output for discovered EFI block devices.
- gbl/efi/src/lib.rs: temporary diagnostic entry point; reports build, image, console and filesystem information, then halts before OS boot.
- gbl/libefi/src/lib.rs: raw EFI protocol lookup helper.
- gbl/libefi/src/protocol/simple_text_output.rs: console mode query and set wrappers.
- gbl/efi/src/diagnostic_fs.rs: writes a report through EFI_SIMPLE_FILE_SYSTEM_PROTOCOL on the loaded-image device only.

## Build artifacts

Signed artifact:
 bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/testkey_signed/gbl.efi
 SHA-256: edc8958b8308cf52d6e6e2d4b3c308c0d8786ee9b27d6b4534e9c72bae9fa4ae

Unsigned artifact:
 bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/gbl.efi
 SHA-256: 90355b258c2eed84f67ab7ceab25c65a210106c89d3a5ea312ab6453def7b24f

## Duo Lite test

The custom EFI application was installed on the test microSD at EFI/BOOT/BOOTAA64.EFI. The previous file was preserved as EFI/BOOT/BOOTAA64.EFI.dualos-backup. The card was mounted at /mnt/dualos-check and labeled GBLTEST.

Verified device output:
```text
**** Generic Bootloader (GBL) ****
Fingerprint: dev/mainline-18/eng.hawk.2026-11-08
VCS info: gbl-mainline: 2392920e8324-dirty
Image path: \\EFI\\BOOT\\BOOTAA64.EFI
Console current mode: 0; available modes: 2
Console mode 0: 80 columns x 25 rows
Console mode 1: 80 columns x 50 rows
Filesystem report: OK: DUALOS-DIAG.TXT written
Diagnostic complete. Halting before OS boot.
```

The generated DUALOS-DIAG.TXT was read back from the card and contained:
```text
DualOS EFI diagnostic
Filesystem: loaded-image device only
Protocol: EFI_SIMPLE_FILE_SYSTEM_PROTOCOL
Report: write completed
```

## Verified versus outstanding

Demonstrated:
- AArch64 GBL development build succeeds.
- Duo Lite boots the custom GBL EFI application from microSD.
- The application accesses the filesystem on its loaded-image device.
- The application creates and writes a report that can be read back.

Not yet demonstrated:
- Booting a complete Android system from microSD through this custom GBL implementation.
- Loading and handing off to a DualOS Android kernel.
- OpenABL integration with this custom boot flow.
- Automatic stock Android fallback when the card is absent.

Important: this build uses a temporary diagnostic entry point that intentionally halts before OS boot. The normal GBL Android/Fuchsia loading flow is bypassed in this build. The diagnostic test is not proof of Android SD boot.

## Next steps

1. Preserve the diagnostic source and build as a known-good checkpoint.
2. Trace the existing GBL Android loading path and identify required boot configuration, image formats and device dependencies.
3. Determine how to locate and load Android boot components from microSD.
4. Implement and test a controlled Android boot handoff.
5. Integrate OpenABL selection and verify stock fallback without modifying internal storage.

## Storage safety

Internal eMMC must remain untouched. Development and testing use microSD unless a future change is explicitly reviewed and approved.

## Preserved diagnostic source patch

The initial GBL diagnostic implementation is preserved in `patches/gbl/duo-lite-diagnostic.patch`.

- Base GBL revision: `2392920e8324`
- Patch includes five source files, including the new `diagnostic_fs.rs`.
- SHA-256: `a5a8f2ebb7bef458e9e495d3b61efec51d82670116f8891531e760c0e0a8ca16`

To apply it to a clean checkout of the matching GBL revision:

```bash
git apply --check patches/gbl/duo-lite-diagnostic.patch
git apply patches/gbl/duo-lite-diagnostic.patch
```

This patch is a development diagnostic checkpoint, not a production DualOS bootloader. Its entry point halts before operating-system boot.
