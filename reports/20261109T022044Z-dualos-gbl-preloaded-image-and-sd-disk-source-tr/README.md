# DualOS GBL preloaded image and SD disk source trace

Report ID: `20261109T022044Z-dualos-gbl-preloaded-image-and-sd-disk-source-tr`

Exit code: `0`

Command: `bash -c 
echo "=== VERIFIED PARTITION LOADING ==="
sed -n "80,180p" gbl/libgbl/src/android_boot/mod.rs
rg -n -C 18 "fn get_verified_partition|pub fn get_verified_partition" gbl/libgbl/src

echo
echo "=== PARTITION BUFFER USE AND FALLBACK ==="
sed -n "90,175p" gbl/libgbl/src/android_boot/mod.rs
rg -n -C 12 "PartitionBuffer::Preloaded|PartitionBuffer::Designated|read_unique_partition" \
  gbl/libgbl/src/android_boot gbl/libgbl/src

echo
echo "=== EFI GBL DISK IMPLEMENTATION ==="
sed -n "1,230p" gbl/efi/src/efi_blocks.rs
sed -n "230,390p" gbl/efi/src/efi_blocks.rs

echo
echo "=== EFI OPS DISK OWNERSHIP AND CONSTRUCTION ==="
rg -n -C 12 "EfiGblDisk|disks:|fn new\\(|GblOps for|impl.*GblOps|read_from_partition|read_partition" \
  gbl/efi/src/ops.rs gbl/efi/src/lib.rs

echo
echo "=== PRELOADED BUFFER PROTOCOL SEMANTICS ==="
sed -n "180,315p" gbl/libefi/src/protocol/gbl_efi_boot_memory.rs
sed -n "740,770p" gbl/efi/src/ops.rs

echo
echo "=== SOURCE DIFF CHECK ==="
git diff --check
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
