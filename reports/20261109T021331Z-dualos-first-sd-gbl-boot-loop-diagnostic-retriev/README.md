# DualOS first SD GBL boot-loop diagnostic retrieval

Report ID: `20261109T021331Z-dualos-first-sd-gbl-boot-loop-diagnostic-retriev`

Exit code: `0`

Command: `bash -c 
PART="/dev/mmcblk0p1"
MNT="/mnt/dualos-check"
EXPECTED="1b806d74-8676-4e78-a90a-9c209e214d81"

ACTUAL="$(sudo blkid -s PARTUUID -o value "$PART" 2>/dev/null)"
echo "PARTUUID: $ACTUAL"
[ "$ACTUAL" = "$EXPECTED" ] || { echo "STOP: unexpected SD partition"; exit 10; }

if mountpoint -q "$MNT"; then
  echo "STOP: mountpoint is already mounted"
  exit 11
fi

sudo mkdir -p "$MNT"
sudo mount -o ro,nosuid,nodev,noexec "$PART" "$MNT" || {
  echo "STOP: read-only mount failed"
  exit 12
}

echo
echo "=== MOUNT ==="
findmnt -rn -T "$MNT" -o SOURCE,FSTYPE,OPTIONS,TARGET

echo
echo "=== DIAGNOSTIC FILES ==="
sudo find "$MNT" -maxdepth 4 -type f \
  \( -iname "DUALOS-DIAG.TXT" -o -iname "*.log" \) \
  -printf "%p (%s bytes)\n"

echo
echo "=== DUALOS-DIAG.TXT ==="
if sudo test -f "$MNT/DUALOS-DIAG.TXT"; then
  sudo cat "$MNT/DUALOS-DIAG.TXT"
elif sudo test -f "$MNT/EFI/BOOT/DUALOS-DIAG.TXT"; then
  sudo cat "$MNT/EFI/BOOT/DUALOS-DIAG.TXT"
else
  echo "Diagnostic file not found at either expected location."
fi

echo
echo "=== ACTIVE EFI HASH ==="
sudo sha256sum "$MNT/EFI/BOOT/BOOTAA64.EFI"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
