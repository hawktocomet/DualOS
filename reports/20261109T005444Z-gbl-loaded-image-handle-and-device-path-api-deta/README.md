# GBL loaded-image handle and device-path API details

Report ID: `20261109T005444Z-gbl-loaded-image-handle-and-device-path-api-deta`

Exit code: `0`

Command: `bash -c 
cd bootable/libbootloader || exit 1

echo "===== EFI ENTRY AND IMAGE HANDLE APIs ====="
rg -n -C 5 "struct EfiEntry|impl EfiEntry|image_handle|loaded_image" gbl/libefi/src

echo
echo "===== PROTOCOL POINTER AND HANDLE APIs ====="
rg -n -C 5 "fn interface_ptr|fn interface\\(|struct DeviceHandle|impl DeviceHandle|open_protocol" gbl/libefi/src/protocol.rs gbl/libefi/src

echo
echo "===== DEVICE PATH UTILITY ====="
rg -n -C 10 "fn get_device_path|pub.*get_device_path|loaded_image_path" gbl/efi/src/utils.rs

echo
echo "===== DEVICE PATH TYPE AND END NODE CONSTANTS ====="
rg -n "EFI_DEVICE_PATH_TYPE_END|END_ENTIRE|struct EfiDevicePathProtocol|type_:" gbl/libefi_types

echo
echo "===== BLOCK HANDLE COLLECTION API ====="
rg -n -C 5 "struct HandleBuffer|fn handles\\(|locate_handle_buffer_by_protocol" gbl/libefi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
