# Diagnose lpunpack slot option mismatch

Report ID: `20261109T025314Z-diagnose-lpunpack-slot-option-mismatch`

Exit code: `0`

Command: `bash -lc 
echo "========== BINARY IDENTITY =========="
command -v lpunpack
readlink -f "$(command -v lpunpack)"
pacman -Qo "$(command -v lpunpack)" 2>&1
pacman -Qi android-tools 2>&1 | head -n 45

echo
echo "========== OPTION PARSER SOURCE / STRINGS =========="
strings "$(command -v lpunpack)" |
  grep -iE -C 2 "slot|partition|unrecognized|Usage:|image=" |
  head -n 100

echo
echo "========== PACKAGE FILES =========="
pacman -Ql android-tools 2>/dev/null |
  grep -E "lpunpack|lpmetadata|partition_tools" |
  head -n 100

echo
echo "DIAGNOSTIC ONLY: no extraction attempted"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
