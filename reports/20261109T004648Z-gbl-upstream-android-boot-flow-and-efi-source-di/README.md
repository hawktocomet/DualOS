# GBL upstream Android boot flow and EFI source-disk mapping

Report ID: `20261109T004648Z-gbl-upstream-android-boot-flow-and-efi-source-di`

Exit code: `0`

Command: `bash -c 
GBL=bootable/libbootloader
cd "$GBL" || exit 1

echo "===== PINNED REVISION AND LOCAL CHANGES ====="
git rev-parse HEAD
git status --short
echo
git diff --stat
echo

echo "===== UPSTREAM EFI ENTRYPOINT ====="
git show HEAD:gbl/efi/src/lib.rs | sed -n "65,260p"

echo
echo "===== UPSTREAM EFI DISK DISCOVERY ====="
git show HEAD:gbl/efi/src/efi_blocks.rs | sed -n "115,205p"

echo
echo "===== UPSTREAM LOADED IMAGE HELPERS ====="
git show HEAD:gbl/efi/src/utils.rs | sed -n "40,85p"

echo
echo "===== UPSTREAM OPS CONSTRUCTION AND DISK ACCESS ====="
git grep -n -C 5 "Ops::new\\|find_block_devices\\|get_target_os" HEAD -- gbl/efi/src

echo
echo "===== DEVICE PATH IMPLEMENTATION ====="
git grep -n -C 5 "struct DevicePathProtocol\\|impl.*DevicePathProtocol\\|DevicePathNode\\|device_path" HEAD -- gbl/libefi/src/protocol/device_path.rs

echo
echo "===== LOADED IMAGE DEVICE HANDLE API ====="
git show HEAD:gbl/libefi/src/protocol/loaded_image.rs | sed -n "35,75p"

echo
echo "===== ANDROID PARTITION RESOLUTION ====="
git grep -n -C 5 "partition_io\\|find_partition\\|get_partition_buffer\\|disks()" HEAD -- gbl/libgbl/src

echo
echo "===== END ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
