# DualOS GBL device-path representation before SD filter patch

Report ID: `20261109T012811Z-dualos-gbl-device-path-representation-before-sd-`

Exit code: `1`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

echo "===== DEVICE PATH PROTOCOL ====="
sed -n "1,135p" gbl/libefi/src/protocol/device_path.rs

echo
echo "===== EFI DEVICE PATH TYPES ====="
rg -n -C 5 "EfiDevicePathProtocol|EfiDevicePathNode|DeviceHandle\\(" gbl/libefi/src gbl/libefi_types

echo
echo "===== LOADED IMAGE ACCESS AND APP INITIALIZATION ====="
sed -n "100,220p" gbl/efi/src/lib.rs

echo
echo "===== BLOCK IO MEDIA DEFINITION ====="
rg -n -C 8 "struct EfiBlockIoMedia|logical_partition" gbl/libefi
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
