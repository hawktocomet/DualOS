# DualOS GBL SD filter media and loaded-image API check

Report ID: `20261109T012848Z-dualos-gbl-sd-filter-media-and-loaded-image-api-`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

echo "===== BLOCK IO MEDIA TYPE ====="
rg -n -C 12 "EfiBlockIoMedia|logical_partition" gbl/libefi_types/src gbl/libefi_types/include

echo
echo "===== LOADED IMAGE PROTOCOL USES ====="
rg -n -C 8 "LoadedImageProtocol|image_handle\\(" gbl/libefi/src gbl/efi/src

echo
echo "===== DEVICE HANDLE DEFINITION AND TRAITS ====="
rg -n -C 5 "pub struct DeviceHandle|struct DeviceHandle|impl.*DeviceHandle" gbl/libefi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
