# DualOS GBL SD source-device disk-filter source trace

Report ID: `20261109T012622Z-dualos-gbl-sd-source-device-disk-filter-source-t`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

echo "===== PINNED SOURCE AND WORKTREE ====="
git rev-parse HEAD
git status --short

echo
echo "===== DISK DISCOVERY AND CONSTRUCTION ====="
rg -n -C 10 "find_block_devices|EfiGblDisk|logical_partition|BlockIoProtocol" gbl/efi gbl/libefi

echo
echo "===== LOADED IMAGE AND DEVICE PATH ====="
rg -n -C 10 "LoadedImageProtocol|device_handle\\(|EfiDevicePathNodeIter|DevicePathProtocol" gbl/efi gbl/libefi

echo
echo "===== DISK LIST AND GBL OPS INITIALIZATION ====="
rg -n -C 12 "GblOps|disks:|\\.disks\\(|android_main|expected_os|get_target_os" gbl/efi gbl/libgbl

echo
echo "===== DEVICE PATH NODE IMPLEMENTATION ====="
sed -n "1,280p" gbl/libefi/src/protocol/device_path.rs

echo
echo "===== BLOCK IO MEDIA IMPLEMENTATION ====="
sed -n "1,240p" gbl/libefi/src/protocol/block_io.rs
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
