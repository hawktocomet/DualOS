# GBL diagnostic fresh build and artifact verification

Report ID: `20261109T010411Z-gbl-diagnostic-fresh-build-and-artifact-verifica`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== WRAPPER TARGET CONFIGURATION ====="
sed -n "1,240p" bootable/libbootloader/gbl/bazel.sh

echo
echo "===== BUILD ====="
./bootable/libbootloader/gbl/bazel.sh build --extra_toolchains=@gbl//toolchain
BUILD_STATUS=$?
echo "BUILD_EXIT_CODE=$BUILD_STATUS"

echo
echo "===== DEVELOPMENT EFI ====="
find out/bazel/output_user_root -type f -name "gbl_aarch64_dev.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; -exec sha256sum {} \;

echo
echo "===== SIGNED EFI ====="
find out/bazel/output_user_root -type f -path "*/testkey_signed/gbl.efi" \
  -exec stat -c "%n | %s bytes | %y" {} \; -exec sha256sum {} \;

exit "$BUILD_STATUS"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
