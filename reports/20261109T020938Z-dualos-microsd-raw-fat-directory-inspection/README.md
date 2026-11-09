# DualOS microSD raw FAT directory inspection

Report ID: `20261109T020938Z-dualos-microsd-raw-fat-directory-inspection`

Exit code: `0`

Command: `bash -c 
SD_PART="/dev/mmcblk0p1"

echo "=== MTOOLS AVAILABILITY ==="
if command -v mdir >/dev/null 2>&1; then
  echo "mdir: $(command -v mdir)"
  echo
  echo "=== RAW ROOT DIRECTORY ==="
  sudo mdir -a -i "$SD_PART" ::/
  echo
  echo "=== EXPECTED EFI DIRECTORY ==="
  sudo mdir -a -i "$SD_PART" ::/EFI/BOOT
else
  echo "mdir is not installed; raw directory listing unavailable."
fi

echo
echo "=== MOUNTED FILE VIEW ==="
sudo find /mnt/dualos-check -xdev -printf "%y %P\n" 2>&1 | sort

echo
echo "=== FAT METADATA SUMMARY ==="
sudo fsck.fat -vn "$SD_PART" 2>&1 | tail -n 35
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
