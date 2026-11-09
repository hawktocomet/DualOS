# Recover known-good B Kerler Firehose read procedure

Report ID: `20261109T023914Z-recover-known-good-b-kerler-firehose-read-proced`

Exit code: `0`

Command: `bash -lc 
ROOT="/home/hawk/duo_edl/edl"

echo "========== SAVED FIREHOSE PROCEDURE =========="
for F in \
  /home/hawk/duo_forensics/firehose_method_minimal.txt \
  /home/hawk/duo_forensics/firehose_method_recovery_20261101_235248.txt \
  /home/hawk/duo_forensics/abl_a_firehose_readback.log \
  /home/hawk/duo_forensics/firehose_abl_write_prep_20261101_235214.txt \
  "$ROOT/README.md"
do
  if [ -f "$F" ]; then
    echo
    echo "----- $F -----"
    sed -n "1,220p" "$F"
  fi
done

echo
echo "========== CLIENT VERSION / HELP (NO DEVICE ACCESS) =========="
"$ROOT/.venv/bin/edl" --help 2>&1 | head -n 160

echo
echo "========== INSTALLED PACKAGE VERSION =========="
"$ROOT/.venv/bin/python" -m pip show edlclient 2>&1 | head -n 35

echo
echo "INSPECTION ONLY: no USB connection or device commands"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
