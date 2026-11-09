# Duo Lite Firehose read-command provenance audit

Report ID: `20261109T023430Z-duo-lite-firehose-read-command-provenance-audit`

Exit code: `0`

Command: `bash -lc 
ROOTS=(
  /home/hawk/duo_forensics
  /home/hawk/duo_edl
)

echo "========== EXISTING EDL READ SCRIPTS =========="
find "${ROOTS[@]}" -maxdepth 5 -type f \
  \( -iname "*.sh" -o -iname "*.py" -o -iname "*.txt" -o -iname "*.log" \) \
  -size -5M -print0 2>/dev/null |
while IFS= read -r -d "" FILE; do
  MATCHES=$(grep -nEi \
    "edl\.py|firehose|read.*partition|partition.*read|--memory|super_a|super " \
    "$FILE" 2>/dev/null)
  if [ -n "$MATCHES" ]; then
    echo
    echo "----- $FILE -----"
    printf "%s\n" "$MATCHES" | head -n 60
  fi
done

echo
echo "========== FIREHOSE TOOL ENTRY POINTS =========="
find /home/hawk/duo_edl /home/hawk/duo_forensics \
  -maxdepth 5 -type f \
  \( -name "edl.py" -o -name "edl" -o -name "*firehose*.py" \) \
  -printf "%p\n" 2>/dev/null | sort -u

echo
echo "AUDIT COMPLETE; NO DEVICE COMMANDS EXECUTED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
