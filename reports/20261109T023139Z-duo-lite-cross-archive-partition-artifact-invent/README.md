# Duo Lite cross-archive partition artifact inventory

Report ID: `20261109T023139Z-duo-lite-cross-archive-partition-artifact-invent`

Exit code: `0`

Command: `bash -lc 
ROOTS=(
  /home/hawk/duolite-abl-backup
  /home/hawk/duolite-dt
  /home/hawk/duo_sd_backups
  /home/hawk/duo_sd_boot_handoff
  /home/hawk/duo_forensics
  /home/hawk/duo_edl
)

for ROOT in "${ROOTS[@]}"; do
  echo
  echo "========== $ROOT =========="
  if [ ! -e "$ROOT" ]; then
    echo "NOT FOUND"
    continue
  fi

  find "$ROOT" -maxdepth 5 -type f \
    \( -size +8M -o \
       -iname "*.xml" -o -iname "*.gpt" -o \
       -iname "*.bin" -o -iname "*.img" -o \
       -iname "*.mbn" -o -iname "*.elf" -o \
       -iname "*partition*" -o -iname "*rawprogram*" -o \
       -iname "*backup*" -o -iname "*manifest*" \) \
    -printf "%s bytes  %p\n" 2>/dev/null |
    sort -nr | head -n 250
done

echo
echo "========== ARCHIVE FILE =========="
if [ -f /home/hawk/duo-analysis-focused.tar.gz ]; then
  tar -tzf /home/hawk/duo-analysis-focused.tar.gz 2>/dev/null |
    grep -Ei "partition|rawprogram|patch[0-9]|gpt|boot_[ab]|system|vendor|vbmeta|super|backup" |
    head -n 200
fi

echo
echo "INVENTORY COMPLETE; NO FILES MODIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
