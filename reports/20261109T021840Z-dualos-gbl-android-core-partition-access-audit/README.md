# DualOS GBL Android core partition access audit

Report ID: `20261109T021840Z-dualos-gbl-android-core-partition-access-audit`

Exit code: `0`

Command: `bash -c 
echo "=== ANDROID AND PARTITION SOURCE FILES ==="
rg --files gbl/libgbl/src | rg "android|partition|slot"

echo
echo "=== ANDROID MAIN IMPLEMENTATION ==="
rg -n -C 8 "android_main|load_boot_images|load.*image|LoadPartition" gbl/libgbl/src

echo
echo "=== GBL PARTITION BUFFER ADAPTER ==="
sed -n "740,775p" gbl/efi/src/ops.rs
sed -n "1,280p" gbl/efi/src/efi_blocks.rs

echo
echo "=== PARTITION BUFFER PROTOCOL CONTRACT ==="
rg -n -C 5 "gbl_get_partition_buffer|GblEfiPartitionBuffer|PARTITION_BUFFER" gbl/libefi/src gbl/efi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
