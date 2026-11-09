# Duo Lite Android boot-chain capture completeness

Report ID: `20261109T023217Z-duo-lite-android-boot-chain-capture-completeness`

Exit code: `0`

Command: `bash -lc 
ROOT=/home/hawk/duo_forensics

for DIR in \
  "$ROOT/android_boot_chain_20261102_094334" \
  "$ROOT/recovery_backups_20261102" \
  "$ROOT/abl_update_20261102"; do
  echo
  echo "========== $DIR =========="
  if [ -d "$DIR" ]; then
    find "$DIR" -maxdepth 3 -type f \
      -printf "%s bytes  %p\n" | sort -k2
  else
    echo "NOT FOUND"
  fi
done

echo
echo "========== FORENSICS ARCHIVE PARTITION-LIKE FILES =========="
ARCHIVE="$ROOT/DUO_FORENSICS_20261005_075018.tar.gz"
if [ -f "$ARCHIVE" ]; then
  tar -tzf "$ARCHIVE" 2>/dev/null |
    grep -Ei "(^|/)(system|vendor|product|odm|system_ext|super|vbmeta|metadata|userdata|persist|modem|boot|vendor_boot|init_boot|dtbo)(_[ab])?(\.|/|$)" |
    head -n 250
fi

echo
echo "========== COMPLETE =========="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
