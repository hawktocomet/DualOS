# DualOS first SD GBL device boot diagnostic

Report ID: `20261109T011814Z-dualos-first-sd-gbl-device-boot-diagnostic`

Exit code: `1`

Command: `bash -c 
CARD=/mnt/dualos-check
REPORT="$CARD/DUALOS-DIAG.TXT"

echo "===== SD CARD MOUNT ====="
findmnt "$CARD"
lsblk -o NAME,SIZE,FSTYPE,LABEL,MOUNTPOINTS /dev/mmcblk0

echo
echo "===== DIAGNOSTIC FILE ====="
if [ -f "$REPORT" ]; then
  stat "$REPORT"
  sha256sum "$REPORT"
  echo
  cat "$REPORT"
  echo
  echo "===== END FILE ====="
else
  echo "DUALOS-DIAG.TXT NOT FOUND"
  echo "SD root contents:"
  ls -la "$CARD"
fi

echo
echo "===== EFI BOOT FILE ====="
sha256sum "$CARD/EFI/BOOT/BOOTAA64.EFI"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
