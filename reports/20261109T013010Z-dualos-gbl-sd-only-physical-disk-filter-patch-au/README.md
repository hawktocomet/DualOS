# DualOS GBL SD-only physical disk filter patch audit

Report ID: `20261109T013010Z-dualos-gbl-sd-only-physical-disk-filter-patch-au`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2
echo "===== WORKTREE STATUS ====="
git status --short
echo
echo "===== DEVICE PATH DIFF ====="
git diff -- gbl/libefi/src/protocol/device_path.rs
echo
echo "===== DISK FILTER DIFF ====="
git diff -- gbl/efi/src/efi_blocks.rs
echo
echo "===== PATCH WHITESPACE CHECK ====="
git diff --check
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
