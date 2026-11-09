# GBL SD device-path diagnostic build verification 2

Report ID: `20261109T010213Z-gbl-sd-device-path-diagnostic-build-verification`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1
echo "===== REVISION ====="
git -C bootable/libbootloader rev-parse HEAD
echo
echo "===== DIAGNOSTIC SOURCE ====="
sed -n "1,260p" bootable/libbootloader/gbl/efi/src/diagnostic_fs.rs
echo
echo "===== BUILD ARTIFACTS ====="
find bootable/libbootloader/bazel-bin -type f \( -iname "*.efi" -o -iname "*.EFI" \) -printf "%p %s bytes\n" 2>/dev/null | tail -40
echo
echo "===== WORKTREE ====="
git -C bootable/libbootloader status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
