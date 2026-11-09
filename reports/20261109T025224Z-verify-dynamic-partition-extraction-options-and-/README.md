# Verify dynamic partition extraction options and metadata slots

Report ID: `20261109T025224Z-verify-dynamic-partition-extraction-options-and-`

Exit code: `0`

Command: `bash -lc 
IMG="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"
OUT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super-unpacked"

echo "========== LPDUMP OPTIONS =========="
lpdump --help 2>&1 | head -n 120

echo
echo "========== LPUNPACK OPTIONS =========="
lpunpack --help 2>&1 | head -n 120

echo
echo "========== METADATA SLOT INSPECTION =========="
for SLOT in 0 1 2; do
  echo
  echo "----- SLOT $SLOT -----"
  lpdump --slot="$SLOT" "$IMG" 2>&1 | head -n 100
done

echo
echo "========== EXTRACTION DESTINATION =========="
if [ -e "$OUT" ]; then
  echo "WARNING: destination already exists; will not overwrite"
  ls -lah "$OUT" | head -n 60
else
  echo "Destination is clear: $OUT"
fi
df -h "$(dirname "$OUT")"

echo
echo "INSPECTION ONLY: no partitions extracted and no source files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
