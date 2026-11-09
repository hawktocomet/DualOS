# DualOS SD boot-loop target selection and partition audit

Report ID: `20261109T021425Z-dualos-sd-boot-loop-target-selection-and-partiti`

Exit code: `0`

Command: `bash -c 
echo "=== TARGET SELECTION IMPLEMENTATION ==="
rg -n -C 12 "fn get_target_os|get_target_os\\(|enum.*TargetOs|Android" \
  bootable/libbootloader/gbl/efi/src \
  bootable/libbootloader/gbl/libefi/src 2>/dev/null | head -n 260

echo
echo "=== GBL DISK ENUMERATION ==="
rg -n -C 8 "struct EfiGblDisk|impl EfiGblDisk|find_block_devices|logical_partition|partition" \
  bootable/libbootloader/gbl/efi/src/efi_blocks.rs \
  bootable/libbootloader/gbl/efi/src 2>/dev/null | head -n 260

echo
echo "=== SD PARTITION TABLE (READ ONLY) ==="
lsblk -o NAME,TYPE,SIZE,FSTYPE,LABEL,PARTUUID,PARTTYPE,MOUNTPOINTS /dev/mmcblk0
sudo sfdisk --dump /dev/mmcblk0 2>&1
echo
echo "=== FILESYSTEM IDENTITIES ==="
sudo blkid /dev/mmcblk0 /dev/mmcblk0p1 2>&1
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
