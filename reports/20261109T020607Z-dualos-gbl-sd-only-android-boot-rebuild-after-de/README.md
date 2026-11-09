# DualOS GBL SD-only Android boot rebuild after DevicePathText fix

Report ID: `20261109T020607Z-dualos-gbl-sd-only-android-boot-rebuild-after-de`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2
printf "\n=== SOURCE CHECK ===\n"
grep -n -A 2 "let selected_disk_path" gbl/efi/src/efi_blocks.rs
git diff --check
printf "\n=== AARCH64 EFI BUILD ===\n"
./gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
