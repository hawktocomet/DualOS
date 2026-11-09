# GBL device-path ancestry matching feasibility

Report ID: `20261109T005219Z-gbl-device-path-ancestry-matching-feasibility`

Exit code: `0`

Command: `bash -c 
GBL=bootable/libbootloader
cd "$GBL" || exit 1

echo "===== GENERIC EFI PROTOCOL HANDLE AND POINTER API ====="
rg -n -C 8 "pub struct Protocol|impl.*Protocol|fn interface_ptr|fn handle\\(" gbl/libefi/src

echo
echo "===== DEVICE PATH HEADER DEFINITIONS ====="
rg -n -C 8 \
  "struct EfiDevicePathProtocol|EFI_DEVICE_PATH_TYPE_END|EFI_END_OF_HARDWARE_DEVICE_PATH_SUB_TYPE_END_ENTIRE_DEVICE_PATH|EFI_MEDIA_DEVICE_PATH_SUB_TYPE_HARD_DRIVE" \
  gbl/libefi_types gbl/libefi/src

echo
echo "===== DEVICE PATH NODE PARSING AND COMPARISON HELPERS ====="
rg -n -C 5 \
  "EfiDevicePathNodeIter|from_bytes|byte_add|device.path.*equal|device.path.*prefix|DevicePath.*PartialEq" \
  gbl/libefi/src gbl/libefi_types

echo
echo "===== BLOCK IO MEDIA PARTITION IDENTIFICATION ====="
rg -n -C 5 "logical_partition|struct EfiBlockIoMedia|EfiBlockIoMedia" \
  gbl/libefi/src gbl/libefi_types

echo
echo "===== AVAILABLE EFI BOOT SERVICE WRAPPERS ====="
rg -n -C 4 "locate_handle|locate_device|LocateDevicePath|open_protocol" \
  gbl/libefi/src

echo
echo "===== END ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
