# GBL source-device affinity and Android partition resolution

Report ID: `20261109T004424Z-gbl-source-device-affinity-and-android-partition`

Exit code: `0`

Command: `bash -c 
ROOT=bootable/libbootloader/gbl

echo "===== REVISION AND WORKTREE ====="
git -C bootable/libbootloader rev-parse HEAD
git -C bootable/libbootloader status --short

echo
echo "===== GBL EFI OPS: CONSTRUCTION AND OS INPUT ====="
rg -n -C 12 \
  "struct Ops|impl.*Ops|Ops::new|expected_os|self\.os|os:" \
  "$ROOT/efi/src"

echo
echo "===== GBL EFI: BLOCK DEVICE DISCOVERY ====="
rg -n -C 12 \
  "find_block_devices|GblDisk|BlockIoProtocol|DevicePathProtocol|LoadedImageProtocol" \
  "$ROOT/efi/src"

echo
echo "===== GBL EFI: LOADED IMAGE DEVICE PATH ====="
rg -n -C 12 \
  "loaded_image|device_handle|device_path|file_path|LoadedImage" \
  "$ROOT/efi/src"

echo
echo "===== SHARED GBL: ANDROID BOOT ENTRY AND OS DETECTION ====="
rg -n -C 10 \
  "android_main|expected_os|Os::Android|Android.*boot|boot_android" \
  "$ROOT/libgbl/src"

echo
echo "===== SHARED GBL: PARTITION LOOKUP AND DISK SELECTION ====="
rg -n -C 10 \
  "find_partition|find.*partition|get_partition|partition.*name|disks\(\)|\.disks|GblDisk|PartitionName" \
  "$ROOT/libgbl/src" "$ROOT/storage"

echo
echo "===== PARTITION BUFFER IMPLEMENTATION ====="
rg -n -C 12 \
  "get_partition_buffer|gbl_get_partition_buffer|GetPartitionBuffer|PartitionBuffer" \
  "$ROOT"

echo
echo "===== ANDROID BOOT FLOW DOCUMENTATION ====="
sed -n "1,240p" "$ROOT/docs/gbl_android_flow.md"

echo
echo "===== END OF REPORT ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
