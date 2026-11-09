# Inspect Firehose archive storage and duplicate data

Report ID: `20261109T025857Z-inspect-firehose-archive-storage-and-duplicate-d`

Exit code: `0`

Command: `bash -lc 
BASE="/home/hawk"

echo "========== FILESYSTEM =========="
df -h "$BASE"

echo
echo "========== SOURCE DIRECTORY BREAKDOWN =========="
for DIR in \
  "$BASE/duo_edl" \
  "$BASE/duo_forensics/android_boot_chain_20261102_094334" \
  "$BASE/duo_forensics/qcs6125_loader_test" \
  "$BASE/duo_forensics/recovery_probe/bkerler-edl"; do
  if [ -e "$DIR" ]; then
    echo
    echo "--- $DIR ---"
    du -h --max-depth=2 "$DIR" 2>/dev/null | sort -h | tail -n 35
  fi
done

echo
echo "========== EDL CHECKOUT TOP-LEVEL =========="
du -h --max-depth=1 "$BASE/duo_edl/edl" 2>/dev/null | sort -h

echo
echo "========== EDL EXECUTABLES AND LOADER =========="
ls -lh \
  "$BASE/duo_edl/edl/.venv/bin/edl" \
  "$BASE/duo_edl/edl/edl.py" \
  "$BASE/duo_forensics/qcs6125_loader_test"/* \
  2>/dev/null

echo
echo "========== KNOWN SUPER IMAGE =========="
stat -c "%n %s bytes" \
  "$BASE/duo_forensics/android_boot_chain_20261102_094334/super.img" \
  2>/dev/null
sha256sum \
  "$BASE/duo_forensics/android_boot_chain_20261102_094334/super.img" \
  2>/dev/null

echo
echo "========== EXISTING ARCHIVE DESTINATION =========="
if [ -e "$BASE/duo_forensics/firehose-recovery-archive" ]; then
  du -sh "$BASE/duo_forensics/firehose-recovery-archive"
else
  echo "No archive created; safe to retry after planning."
fi

echo
echo "INSPECTION ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
