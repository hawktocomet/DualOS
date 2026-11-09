# DualOS microSD FAT and block-device read-only integrity check

Report ID: `20261109T020852Z-dualos-microsd-fat-and-block-device-read-only-in`

Exit code: `0`

Command: `bash -c 
SD_PART="/dev/mmcblk0p1"
MNT="/mnt/dualos-check"

echo "=== BLOCK DEVICE TOPOLOGY ==="
lsblk -o NAME,TYPE,SIZE,PKNAME,TRAN,RM,FSTYPE,LABEL,PARTUUID,MOUNTPOINTS

echo
echo "=== MOUNT OPTIONS ==="
findmnt -rn -T "$MNT" -o SOURCE,FSTYPE,OPTIONS,TARGET

echo
echo "=== RAW DIRECTORY LISTING ==="
sudo ls -la "$MNT"
sudo du -ahx "$MNT" 2>&1 | head -n 100

echo
echo "=== READ-ONLY FAT CHECK ==="
if command -v fsck.fat >/dev/null 2>&1; then
  sudo fsck.fat -vn "$SD_PART"
else
  echo "fsck.fat unavailable; no filesystem check run."
fi

echo
echo "=== DEVICE IDENTITY ==="
sudo blkid "$SD_PART"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
