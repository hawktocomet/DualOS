# DualOS GBL SD-only observable Android boot AArch64 build

Report ID: `20261109T020515Z-dualos-gbl-sd-only-observable-android-boot-aarch`

Exit code: `1`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2
./gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
