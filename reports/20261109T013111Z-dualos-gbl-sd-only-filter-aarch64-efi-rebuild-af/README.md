# DualOS GBL SD-only filter AArch64 EFI rebuild after Clippy fix

Report ID: `20261109T013111Z-dualos-gbl-sd-only-filter-aarch64-efi-rebuild-af`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2
./gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
