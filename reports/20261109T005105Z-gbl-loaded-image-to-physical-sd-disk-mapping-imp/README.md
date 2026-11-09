# GBL loaded-image to physical SD disk mapping implementation audit

Report ID: `20261109T005105Z-gbl-loaded-image-to-physical-sd-disk-mapping-imp`

Exit code: `0`

Command: `bash -c 
GBL=bootable/libbootloader
cd "$GBL" || exit 1

echo "===== REVISION AND CURRENT LOCAL PATCHES ====="
git rev-parse HEAD
git status --short
git diff -- gbl/efi/src/lib.rs gbl/efi/src/efi_blocks.rs gbl/libefi/src/protocol/device_path.rs
echo

echo "===== BLOCK DISCOVERY: HANDLE AND DEVICE PATH AVAILABILITY ====="
sed -n "115,215p" gbl/efi/src/efi_blocks.rs
echo

echo "===== LOADED IMAGE DEVICE HANDLE ====="
sed -n "25,75p" gbl/libefi/src/protocol/loaded_image.rs
echo

echo "===== DEVICE PATH PROTOCOL INTERFACE ====="
sed -n "35,115p" gbl/libefi/src/protocol/device_path.rs
echo

echo "===== DEVICE PATH RAW INTERFACE ACCESSORS ====="
rg -n -C 5 "fn interface_ptr|interface_ptr\\(" gbl/libefi/src
echo

echo "===== BOOT SERVICES: HANDLE AND DEVICE-PATH LOOKUP ====="
rg -n -C 5 "locate_device_path|LocateDevicePath|locate_handle_buffer_by_protocol|handle_protocol_raw|open_protocol_raw" gbl/libefi/src
echo

echo "===== DEVICE PATH TYPES AND NODE STRUCTURES ====="
rg -n -C 5 "struct EfiDevicePathProtocol|EFI_DEVICE_PATH_TYPE_END|END_ENTIRE|END_INSTANCE|DevicePath" gbl/libefi_types gbl/libefi/src/protocol/device_path.rs
echo

echo "===== EFI OPS DISK SLICE CONSTRUCTION ====="
sed -n "105,140p" gbl/efi/src/lib.rs
rg -n -C 8 "pub fn new|struct Ops|disks:" gbl/efi/src/ops.rs
echo

echo "===== CURRENT DIAGNOSTIC ENTRYPOINT CHANGES ====="
git diff -- gbl/efi/src/lib.rs
echo

echo "===== END ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
