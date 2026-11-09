# DualOS GBL exact disk discovery implementation

Report ID: `20261109T012703Z-dualos-gbl-exact-disk-discovery-implementation`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

echo "===== EFI DISK IMPLEMENTATION ====="
sed -n "1,360p" gbl/efi/src/efi_blocks.rs

echo
echo "===== EFI ENTRY AND DISK DISCOVERY CALLERS ====="
rg -n -C 18 "efi_blocks|EfiGblDisk|find_block|find_disk|disks" gbl/efi/src --glob "*.rs" --glob "!diagnostic_fs.rs"

echo
echo "===== DEVICE PATH NODE AND PROTOCOL API ====="
rg -n "pub fn|pub\\(crate\\) fn|struct EfiDevicePathNodeIter|impl.*DevicePath" gbl/libefi/src/protocol/device_path.rs
sed -n "1,110p" gbl/libefi/src/protocol/loaded_image.rs
sed -n "420,485p" gbl/libefi/src/protocol.rs
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
