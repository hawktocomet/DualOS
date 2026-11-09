# DualOS GBL SD source-device disk-filter implementation audit

Report ID: `20261109T012555Z-dualos-gbl-sd-source-device-disk-filter-implemen`

Exit code: `129`

Command: `bash -c 
echo "===== PINNED SOURCE ====="
git -C bootable/libbootloader rev-parse HEAD
git -C bootable/libbootloader status --short

echo
echo "===== DISK DISCOVERY AND CONSTRUCTION ====="
rg -n -C 8 "find_block_devices|EfiGblDisk|struct GblDisk|impl.*GblDisk|BlockIoProtocol|logical_partition" \
  gbl/efi gbl/libgbl gbl/libefi

echo
echo "===== LOADED IMAGE AND DEVICE PATH ====="
rg -n -C 10 "LoadedImageProtocol|device_handle\\(|get_device_path|EfiDevicePathNodeIter|DevicePathProtocol" \
  gbl/efi gbl/libefi

echo
echo "===== DISK LIST AND OPS CONSTRUCTION ====="
rg -n -C 12 "GblOps|expected_os|\\.disks\\(|disks:|android_main|efi_android_load|get_target_os" \
  gbl/efi gbl/libgbl

echo
echo "===== DEVICE PATH IMPLEMENTATION ====="
sed -n "1,280p" gbl/libefi/src/protocol/device_path.rs

echo
echo "===== BLOCK IO MEDIA STRUCTURE ====="
sed -n "1,240p" gbl/libefi/src/protocol/block_io.rs

echo
echo "===== LOCAL DIFF SUMMARY ====="
git diff --stat
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
