# GBL SD-only boot implementation context

Report ID: `20261109T005401Z-gbl-sd-only-boot-implementation-context`

Exit code: `0`

Command: `bash -c 
GBL=bootable/libbootloader
cd "$GBL" || exit 1

echo "===== REVISION AND WORKTREE ====="
git rev-parse HEAD
git status --short
echo
echo "===== EFI ENTRYPOINT AND OPS CONSTRUCTION ====="
sed -n "1,260p" gbl/efi/src/lib.rs
echo
echo "===== BLOCK DEVICE DISCOVERY ====="
sed -n "1,260p" gbl/efi/src/efi_blocks.rs
echo
echo "===== LOADED IMAGE PROTOCOL ====="
sed -n "1,180p" gbl/libefi/src/protocol/loaded_image.rs
echo
echo "===== DEVICE PATH PROTOCOL ====="
sed -n "1,180p" gbl/libefi/src/protocol/device_path.rs
echo
echo "===== GENERIC PROTOCOL WRAPPER ====="
sed -n "1,180p" gbl/libefi/src/protocol.rs
echo
echo "===== EFI OPS DISK OWNERSHIP AND CONSTRUCTOR ====="
rg -n -C 12 "struct Ops|impl.*Ops|fn new\\(|find_block_devices|disks:" gbl/efi/src/ops.rs gbl/efi/src/lib.rs
echo
echo "===== CURRENT LOCAL DIFF ====="
git diff -- gbl/efi/src/lib.rs gbl/efi/src/efi_blocks.rs gbl/efi/src/ops.rs gbl/libefi/src/protocol/device_path.rs
echo
echo "===== END ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
