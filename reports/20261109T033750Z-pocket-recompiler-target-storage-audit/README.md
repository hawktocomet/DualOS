# pocket-recompiler-target-storage-audit

Report ID: `20261109T033750Z-pocket-recompiler-target-storage-audit`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/pocket-recompiler/target"

echo "========== TARGET DIRECTORY BREAKDOWN =========="
du -xhd2 "$ROOT" 2>/dev/null | sort -h | tail -n 60

echo
echo "========== LARGEST FILES =========="
find "$ROOT" -xdev -type f -printf "%s\t%p\n" 2>/dev/null |
  sort -nr | head -n 40 |
  numfmt --field=1 --to=iec --suffix=B

echo
echo "========== TOP-LEVEL CONTENTS =========="
ls -lah "$ROOT"

echo
echo "READ-ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
