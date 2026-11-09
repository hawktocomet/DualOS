# GBL diagnostic EFI target and artifact verification

Report ID: `20261109T010335Z-gbl-diagnostic-efi-target-and-artifact-verificat`

Exit code: `1`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== EFI TARGET DEFINITIONS ====="
sed -n "1,220p" bootable/libbootloader/gbl/efi/BUILD

echo
echo "===== EXISTING ARTIFACTS ====="
find out/bazel/output_user_root -type f \
  \( -name "gbl_aarch64_dev.efi" -o -path "*/testkey_signed/gbl.efi" \) \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

echo
echo "===== REBUILD DEVELOPMENT TARGET ====="
./bootable/libbootloader/gbl/bazel.sh build \
  --extra_toolchains=@gbl//toolchain \
  @@gbl+//efi:aarch64_dev
BUILD_STATUS=$?
echo "BUILD_EXIT_CODE=$BUILD_STATUS"

echo
echo "===== RESULTING DEVELOPMENT ARTIFACT ====="
find out/bazel/output_user_root -type f -name "gbl_aarch64_dev.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; \
  -exec sha256sum {} \;

exit "$BUILD_STATUS"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
