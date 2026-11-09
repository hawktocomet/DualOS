# DualOS GBL Android loader partition requirements

Report ID: `20261109T021739Z-dualos-gbl-android-loader-partition-requirements`

Exit code: `0`

Command: `bash -c 
echo "=== TARGET SELECTION ==="
sed -n "60,112p" gbl/efi/src/lib.rs

echo
echo "=== ANDROID EFI LOAD ENTRY ==="
sed -n "1,125p" gbl/efi/src/android_boot.rs

echo
echo "=== ANDROID PARTITION LOOKUP AND IMAGE LOADING ==="
rg -n -C 4 "PartitionName|partition_name|find_partition|load_partition|bootimage|BootImage|vendor_boot|init_boot|vbmeta|boot_a|boot_b" \
  gbl/efi/src gbl/libgbl/src

echo
echo "=== DISK ENUMERATION AND PARTITION EXPOSURE ==="
rg -n -C 4 "struct EfiGblDisk|impl.*EfiGblDisk|EfiGblDisk|partition" \
  gbl/efi/src/efi_blocks.rs gbl/efi/src/ops.rs
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
