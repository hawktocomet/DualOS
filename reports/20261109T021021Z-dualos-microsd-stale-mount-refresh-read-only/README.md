# DualOS microSD stale mount refresh read-only

Report ID: `20261109T021021Z-dualos-microsd-stale-mount-refresh-read-only`

Exit code: `0`

Command: `bash -c 
PART="/dev/mmcblk0p1"
MNT="/mnt/dualos-check"
EXPECTED="1b806d74-8676-4e78-a90a-9c209e214d81"

ACTUAL="$(sudo blkid -s PARTUUID -o value "$PART" 2>/dev/null)"
[ "$ACTUAL" = "$EXPECTED" ] || { echo "STOP: PARTUUID mismatch: $ACTUAL"; exit 10; }

echo "=== UNMOUNT ==="
if mountpoint -q "$MNT"; then
  sudo umount "$MNT" || { echo "STOP: unmount failed"; exit 11; }
fi

echo "=== READ-ONLY REMOUNT ==="
sudo mount -o ro "$PART" "$MNT" || { echo "STOP: read-only mount failed"; exit 12; }

echo "=== MOUNT IDENTITY ==="
findmnt -rn -T "$MNT" -o SOURCE,FSTYPE,OPTIONS,TARGET

echo
echo "=== MOUNTED EFI DIRECTORY ==="
sudo find "$MNT" -maxdepth 4 -type f -printf "%p %s bytes\n" | sort

echo
echo "=== ACTIVE EFI HASH ==="
sudo sha256sum "$MNT/EFI/BOOT/BOOTAA64.EFI" || exit 13

echo
echo "=== ROOT LISTING ==="
sudo ls -la "$MNT"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
