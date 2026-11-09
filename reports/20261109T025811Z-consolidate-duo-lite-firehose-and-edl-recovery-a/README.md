# Consolidate Duo Lite Firehose and EDL recovery archive

Report ID: `20261109T025811Z-consolidate-duo-lite-firehose-and-edl-recovery-a`

Exit code: `21`

Command: `bash -lc 
BASE="/home/hawk"
DEST="$BASE/duo_forensics/firehose-recovery-archive"

SOURCES=(
  "$BASE/duo_edl"
  "$BASE/duo_forensics/android_boot_chain_20261102_094334"
  "$BASE/duo_forensics/qcs6125_loader_test"
  "$BASE/duo_forensics/recovery_probe/bkerler-edl"
  "$BASE/duo_edl_report_2.sh"
  "$BASE/duo_fastboot_vars.txt"
)

echo "========== PREFLIGHT =========="
if [ -e "$DEST" ]; then
  echo "ERROR: Archive destination already exists: $DEST"
  echo "Refusing to overwrite it."
  exit 20
fi

TOTAL=0
for SRC in "${SOURCES[@]}"; do
  if [ -e "$SRC" ]; then
    SIZE=$(du -sb "$SRC" | awk "{print \$1}")
    TOTAL=$((TOTAL + SIZE))
    printf "%12s bytes  %s\n" "$SIZE" "$SRC"
  else
    echo "NOT FOUND (skipping): $SRC"
  fi
done

AVAILABLE=$(df -B1 "$BASE" | awk "NR==2 {print \$4}")
echo
echo "Total source size: $TOTAL bytes"
echo "Available space:   $AVAILABLE bytes"

if [ "$TOTAL" -eq 0 ] || [ "$AVAILABLE" -lt "$((TOTAL + TOTAL / 10))" ]; then
  echo "ERROR: Insufficient space or no source data found."
  exit 21
fi

echo
echo "========== COPYING ORIGINALS =========="
mkdir -p "$DEST/home/hawk"

for SRC in "${SOURCES[@]}"; do
  if [ -e "$SRC" ]; then
    REL="${SRC#$BASE/}"
    TARGET="$DEST/home/hawk/$REL"
    mkdir -p "$(dirname "$TARGET")"
    echo "Copying: $REL"
    cp -a "$SRC" "$TARGET"
    RC=$?
    if [ "$RC" -ne 0 ]; then
      echo "ERROR copying $SRC (exit $RC)"
      echo "Partial archive preserved at $DEST"
      exit "$RC"
    fi
  fi
done

echo
echo "========== ARCHIVE NOTES =========="
cat > "$DEST/README.txt" <<EOF
DUO LITE FIREHOSE / EDL RECOVERY ARCHIVE

Purpose:
Preserve the Firehose tooling, programmer loader, raw partition captures,
extracted Android logical partitions, and related research material.

Original source paths are preserved beneath:
home/hawk/

Important captured artifact:
android_boot_chain_20261102_094334/super.img

This archive is a copy. Original source files were not moved or deleted.
No device partitions were written or modified by this archive operation.

The programmer loader and EDL client are retained for reproducibility.
Treat this archive as a recovery/forensic backup.
EOF

echo
echo "========== FILE INVENTORY =========="
find "$DEST" -type f -printf "%P\t%s bytes\n" | sort > "$DEST/FILE-INVENTORY.tsv"
cat "$DEST/FILE-INVENTORY.tsv"

echo
echo "========== SHA256 MANIFEST =========="
(
  cd "$DEST" || exit 30
  find . -type f ! -name SHA256SUMS -print0 |
    sort -z |
    xargs -0 -r sha256sum > SHA256SUMS
)
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR generating SHA256 manifest"
  exit "$RC"
fi

echo
echo "========== ARCHIVE SUMMARY =========="
du -sh "$DEST"
echo "Files: $(find "$DEST" -type f | wc -l)"
echo "SHA256 entries: $(wc -l < "$DEST/SHA256SUMS")"
echo "Archive: $DEST"

echo
echo "========== VERIFY MANIFEST =========="
(
  cd "$DEST" || exit 31
  sha256sum -c SHA256SUMS
)
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR: Manifest verification failed"
  exit "$RC"
fi

echo
echo "FIREHOSE_RECOVERY_ARCHIVE_COMPLETE"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
