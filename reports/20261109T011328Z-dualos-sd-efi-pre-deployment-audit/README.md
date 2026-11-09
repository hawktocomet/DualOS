# DualOS SD EFI pre-deployment audit

Report ID: `20261109T011328Z-dualos-sd-efi-pre-deployment-audit`

Exit code: `0`

Command: `bash -c 
echo "===== DEVICE AND FILESYSTEM ====="
lsblk -o NAME,RM,SIZE,FSTYPE,LABEL,UUID,MOUNTPOINTS /dev/mmcblk0
findmnt /mnt/dualos-check
df -h /mnt/dualos-check

echo
echo "===== SD ROOT CONTENTS ====="
ls -la /mnt/dualos-check

echo
echo "===== EFI FILES ====="
find /mnt/dualos-check -maxdepth 5 -type f \
  \( -iname "*.efi" -o -iname "*.bak" \) \
  -print

echo
echo "===== EXISTING DEFAULT BOOT FILE ====="
if [ -f /mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI ]; then
  stat /mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI
  sha256sum /mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI
  file /mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI
else
  echo "BOOTAA64.EFI not found at standard path"
fi

echo
echo "===== NEW DIAGNOSTIC EFI ====="
sha256sum out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/testkey_signed/gbl.efi
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
