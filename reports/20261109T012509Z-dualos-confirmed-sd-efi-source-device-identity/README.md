# DualOS confirmed SD EFI source-device identity

Report ID: `20261109T012509Z-dualos-confirmed-sd-efi-source-device-identity`

Exit code: `0`

Command: `bash -c 
CARD=/mnt/dualos-check
REPORT="$CARD/DUALOS-DIAG.TXT"
EFI="$CARD/EFI/BOOT/BOOTAA64.EFI"

echo "===== DEVICE IDENTITY ====="
lsblk -o NAME,SIZE,FSTYPE,LABEL,UUID,MOUNTPOINTS /dev/mmcblk0
sudo blkid /dev/mmcblk0p1

echo
echo "===== EFI ARTIFACT ====="
sha256sum "$EFI"
file "$EFI"

echo
echo "===== COMPLETE DEVICE DIAGNOSTIC ====="
cat "$REPORT"

echo
echo "===== SOURCE/PARENT MATCH SUMMARY ====="
grep -E "^(LOADED_IMAGE_DEVICE:|LOADED_IMAGE_HANDLE:|COUNT:|BLOCK #90 |PATH:|BLOCK #91 |END_OF_REPORT)" "$REPORT"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
