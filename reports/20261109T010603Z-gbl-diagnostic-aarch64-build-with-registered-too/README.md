# GBL diagnostic AArch64 build with registered toolchains

Report ID: `20261109T010603Z-gbl-diagnostic-aarch64-build-with-registered-too`

Exit code: `1`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== BUILD DIAGNOSTIC AARCH64 EFI ====="
./bootable/libbootloader/gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev \
  @gbl//efi:gbl_efi_testkey
BUILD_STATUS=$?
echo "BUILD_EXIT_CODE=$BUILD_STATUS"

echo
echo "===== DEVELOPMENT EFI ARTIFACT ====="
find out/bazel/output_user_root -type f -name "gbl_aarch64_dev.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

echo
echo "===== SIGNED EFI ARTIFACT ====="
find out/bazel/output_user_root -type f -path "*/testkey_signed/gbl.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

echo
echo "===== WORKTREE ====="
git -C bootable/libbootloader status --short

exit "$BUILD_STATUS"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
