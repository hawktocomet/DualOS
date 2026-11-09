# DualOS GBL SD-only image source verification

Report ID: `20261109T021917Z-dualos-gbl-sd-only-image-source-verification`

Exit code: `0`

Command: `bash -c 
echo "=== ANDROID MAIN FUNCTION LOCATION ==="
rg -n "android_main" gbl/libgbl/src

echo
echo "=== PARTITION READ IMPLEMENTATIONS ==="
rg -n "impl.*GblDisk|read_unique_partition|read_partition|fn read" \
  gbl/efi/src/efi_blocks.rs gbl/libgbl/src/partition.rs

echo
echo "=== EFI PARTITION BUFFER PROTOCOL CALL ==="
rg -n -C 12 "fn get_partition_buffer|gbl_get_partition_buffer|fn sync_partition_buffer|gbl_sync_partition_buffer" \
  gbl/efi/src/ops.rs gbl/libefi/src/protocol/gbl_efi_boot_memory.rs

echo
echo "=== BOOT IMAGE LOAD REQUESTS ==="
rg -n -C 5 "LoadPartition::|get_partition_buffer\\(" \
  gbl/libgbl/src/android_boot gbl/libgbl/src

echo
echo "=== DISK CONSTRUCTION ==="
sed -n "1,280p" gbl/efi/src/efi_blocks.rs
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
