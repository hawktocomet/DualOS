# DualOS microSD EFI destination layout inspection

Report ID: `20261109T020811Z-dualos-microsd-efi-destination-layout-inspection`

Exit code: `0`

Command: `bash -c 
SD_PART="/dev/mmcblk0p1"
MOUNT_DIR="$(findmnt -rn -S "$SD_PART" -o TARGET | head -n 1)"

echo "Partition: $SD_PART"
echo "Mount: $MOUNT_DIR"
echo
echo "=== ROOT DIRECTORY ==="
sudo find "$MOUNT_DIR" -maxdepth 3 -printf "%y %p\n" 2>/dev/null | sort | head -n 250

echo
echo "=== EFI FILES ==="
sudo find "$MOUNT_DIR" -type f \( -iname "*.efi" -o -iname "BOOTAA64.EFI" \) -printf "%p\n" 2>/dev/null

echo
echo "=== MOUNT IDENTITY ==="
findmnt -rn -T "$MOUNT_DIR" -o SOURCE,FSTYPE,TARGET
sudo blkid "$SD_PART"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
