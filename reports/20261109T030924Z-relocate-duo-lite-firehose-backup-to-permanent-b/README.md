# Relocate Duo Lite Firehose backup to permanent backups

Report ID: `20261109T030924Z-relocate-duo-lite-firehose-backup-to-permanent-b`

Exit code: `0`

Command: `bash -lc 
OLD="/home/hawk/duo_forensics/duolite-firehose-backup"
PARENT="/home/hawk/Backups/Retroid-Pocket-Duo-Lite"
NEW="$PARENT/Firehose-Recovery"

echo "========== PREFLIGHT =========="
if [ ! -d "$OLD" ] || [ ! -f "$OLD/SHA256SUMS" ]; then
  echo "ERROR: source backup or checksum manifest missing"
  exit 20
fi
if [ -e "$NEW" ]; then
  echo "ERROR: destination already exists; refusing overwrite"
  exit 21
fi
mkdir -p "$PARENT"

echo
echo "========== MOVE =========="
mv "$OLD" "$NEW"
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR: move failed"
  exit "$RC"
fi

echo
echo "========== VERIFY ALL CHECKSUMS =========="
VERIFY_LOG="/tmp/duolite-firehose-relocation-check.txt"
(
  cd "$NEW" || exit 30
  sha256sum -c SHA256SUMS > "$VERIFY_LOG" 2>&1
)
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR: checksum verification failed"
  tail -n 30 "$VERIFY_LOG"
  exit 31
fi

echo "All $(wc -l < "$NEW/SHA256SUMS") checksum entries passed."

echo
echo "========== VERIFY SUPER IMAGE =========="
sha256sum "$NEW/duo-lite-captures/raw-partitions/super.img"

echo
echo "========== FINAL LOCATION =========="
du -sh "$NEW"
ls -ld "$NEW"
test ! -e "$OLD" && echo "Old location is clear."
df -h /home/hawk
echo "FIREHOSE_BACKUP_RELOCATION_VERIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
