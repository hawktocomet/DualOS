# Duo Lite Firehose command syntax verification

Report ID: `20261109T023559Z-duo-lite-firehose-command-syntax-verification`

Exit code: `0`

Command: `bash -lc 
echo "========== PRIOR EDL REPORT SCRIPT =========="
if [ -f /home/hawk/duo_edl_report_2.sh ]; then
  grep -nEi -C 5 "edl|firehose|read|dump|sector|partition" \
    /home/hawk/duo_edl_report_2.sh
else
  echo "Not found"
fi

echo
echo "========== EDL CLIENT LOCATIONS =========="
find /home/hawk/duo_edl /home/hawk/duo_forensics \
  -maxdepth 7 -type f \
  \( -name "edl.py" -o -name "edl" -o -name "firehose.py" \) \
  -printf "%p\n" 2>/dev/null | sort -u

echo
echo "========== ORIGINAL READ LOG COMMAND CONTEXT =========="
grep -RniE -m 30 \
  "Dumped sector 1972736|edl.*r |edl.*read|read.*sector" \
  /home/hawk/duo_forensics /home/hawk/duo_edl \
  --include="*.sh" --include="*.py" --include="*.txt" \
  --include="*.log" 2>/dev/null

echo
echo "INSPECTION COMPLETE; NOTHING EXECUTED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
