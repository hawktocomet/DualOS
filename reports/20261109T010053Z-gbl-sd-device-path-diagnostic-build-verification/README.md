# GBL SD device-path diagnostic build verification

Report ID: `20261109T010053Z-gbl-sd-device-path-diagnostic-build-verification`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== PINNED REVISION ====="
git -C bootable/libbootloader rev-parse HEAD

echo
echo "===== DIAGNOSTIC WRITER ====="
sed -n "1,300p" bootable/libbootloader/gbl/efi/src/diagnostic_fs.rs

echo
echo "===== BUILD ====="
./bootable/libbootloader/gbl/bazel.sh build --extra_toolchains=@gbl//toolchain
BUILD_STATUS=$?
echo "BUILD_EXIT_CODE=$BUILD_STATUS"

echo
echo "===== EFI ARTIFACTS ====="
find bootable/libbootloader/bazel-bin -type f \( -iname "*.efi" -o -iname "*.EFI" \) -printf "%p %s bytes\n" 2>/dev/null | tail -40

echo
echo "===== WORKTREE STATUS ====="
git -C bootable/libbootloader status --short

exit "$BUILD_STATUS"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
