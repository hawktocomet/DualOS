# DualOS SD post-boot filesystem integrity audit

Report ID: `20261109T011903Z-dualos-sd-post-boot-filesystem-integrity-audit`

Exit code: `0`

Command: `bash -c 
DEVICE=/dev/mmcblk0p1
CARD=/mnt/dualos-check
BACKUP="$HOME/dualos-sd-backups/20261109T0113Z/BOOTAA64.EFI.original"

echo "===== DEVICE IDENTITY ====="
lsblk -o NAME,MAJ:MIN,SIZE,RO,TYPE,FSTYPE,LABEL,UUID,MOUNTPOINTS /dev/mmcblk0
sudo blkid "$DEVICE"

echo
echo "===== PRESERVED HOST BACKUP ====="
if [ -f "$BACKUP" ]; then
  sha256sum "$BACKUP"
  file "$BACKUP"
else
  echo "HOST BACKUP MISSING"
fi

echo
echo "===== UNMOUNT FOR READ-ONLY CHECK ====="
if mountpoint -q "$CARD"; then
  sudo umount "$CARD"
fi
findmnt "$CARD" || echo "Card unmounted"

echo
echo "===== FAT FILESYSTEM CHECK (NO REPAIR) ====="
sudo fsck.fat -n -v "$DEVICE"
FSCK_STATUS=$?
echo "fsck.fat exit status: $FSCK_STATUS"

echo
echo "===== RECENT KERNEL STORAGE MESSAGES ====="
sudo dmesg --ctime | tail -n 180 | grep -Ei "mmc|vfat|fat-fs|I/O error|buffer I/O|reset|timeout|corrupt|usb|sd" || true

echo
echo "===== PARTITION GEOMETRY ====="
sudo fdisk -l /dev/mmcblk0
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
