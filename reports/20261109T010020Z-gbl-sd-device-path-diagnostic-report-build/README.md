# GBL SD device-path diagnostic report build

Report ID: `20261109T010020Z-gbl-sd-device-path-diagnostic-report-build`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1
echo "===== REVISION ====="
git -C bootable/libbootloader rev-parse HEAD
echo
echo "===== WORKTREE STATUS ====="
git -C bootable/libbootloader status --short
echo
echo "===== DIAGNOSTIC REPORT WRITER DIFF ====="
git -C bootable/libbootloader diff -- gbl/efi/src/diagnostic_fs.rs
echo
echo "===== BUILD ARTIFACTS ====="
find bootable/libbootloader/bazel-bin -type f \( -name "*.efi" -o -name "*.EFI" \) -printf "%p %s bytes\n" 2>/dev/null | tail -30
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
