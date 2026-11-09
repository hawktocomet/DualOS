# GBL AArch64 diagnostic EFI platform build

Report ID: `20261109T010815Z-gbl-aarch64-diagnostic-efi-platform-build`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

./bootable/libbootloader/gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev

BUILD_STATUS=$?
echo "BUILD_EXIT_CODE=$BUILD_STATUS"

echo
echo "===== AARCH64 DEVELOPMENT EFI ====="
find out/bazel/output_user_root -type f -name "gbl_aarch64_dev.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

echo
echo "===== SIGNED EFI ====="
find out/bazel/output_user_root -type f -path "*/testkey_signed/gbl.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

exit "$BUILD_STATUS"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
