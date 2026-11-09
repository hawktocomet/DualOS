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

## Latest SD-only GBL device-selection test

The later SD-only Android GBL build was deployed to the test microSD as `EFI/BOOT/BOOTAA64.EFI`. SHA-256: `4b066ad11e222529a0ad526fa11c20e63ffb1480a819c4a45dbcac71b208ef55`.

The device executed this build. Diagnostics confirmed that the loaded-image device path resolved to the SD card, physical-parent discovery succeeded, other disks were excluded, and GBL returned exactly one selected physical disk (`GBL_DISKS_RETURNED: 1`). The deployment preserved the previous EFI as a rollback copy. Internal eMMC and openABL were not modified.

The boot attempt entered a loop. This demonstrates execution of the modified GBL and SD-only physical-device selection, but not successful Android partition discovery, image loading, or kernel handoff. The cause of the loop is unknown; an absent or invalid Android installation on the SD card is one possibility, not a confirmed diagnosis.

Evidence:
- [SD-only GBL deployment](https://github.com/hawktocomet/DualOS/tree/reports/reports/20261109T021053Z-dualos-deploy-observable-sd-only-android-gbl-efi)
- [First SD GBL boot-loop diagnostic](https://github.com/hawktocomet/DualOS/tree/reports/reports/20261109T021331Z-dualos-first-sd-gbl-boot-loop-diagnostic-retriev)

## Verified versus outstanding

Demonstrated:
- AArch64 GBL development build succeeds.
- The current SD-only GBL build executes on the Duo Lite and selects the SD physical device while excluding other disks.
- Duo Lite boots the custom GBL EFI application from microSD.
- The application accesses the filesystem on its loaded-image device.
- The application creates and writes a report that can be read back.

Not yet demonstrated:
- Discovering and loading the required Android partitions from microSD.
- Booting a complete Android system from microSD through this custom GBL implementation.
- Loading and handing off to a DualOS Android kernel.
- OpenABL integration with this custom boot flow.
- Automatic stock Android fallback when the card is absent.

Important: the earlier diagnostic build uses a temporary entry point that intentionally halts before OS boot. The normal GBL Android/Fuchsia loading flow is bypassed in this build. The diagnostic test is not proof of Android SD boot.

## Next steps

1. Preserve the diagnostic source, SD-only selection changes, and current deployed EFI as rollback/reference artifacts.
2. Establish the Android-on-SD partition and image layout required by the Duo Lite.
3. Trace GBL Android partition discovery, boot configuration, image loading, and device dependencies against that layout.
4. Implement and test a controlled Android boot handoff from SD only.
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
