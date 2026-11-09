# DualOS GBL SD-only physical disk filter AArch64 EFI build

Report ID: `20261109T013036Z-dualos-gbl-sd-only-physical-disk-filter-aarch64-`

Exit code: `1`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

./gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
