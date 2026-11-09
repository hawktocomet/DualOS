# Identify contents of oversized Duo EDL reports

Report ID: `20261109T031409Z-identify-contents-of-oversized-duo-edl-reports`

Exit code: `0`

Command: `bash -lc 
ROOT="/home/hawk/duo_edl/reports"

echo "========== OVERSIZED REPORT FILE TYPES =========="
find "$ROOT" -maxdepth 1 -type f -size +1G -print0 |
while IFS= read -r -d "" F; do
  echo
  echo "--- $(basename "$F")"
  stat -c "size=%s bytes allocated=%b blocks" "$F"
  file -b "$F"
  printf "first 96 bytes: "
  head -c 96 "$F" | od -An -tx1
  printf "last 96 bytes:  "
  tail -c 96 "$F" | od -An -tx1
  echo "beginning sample:"
  head -c 2048 "$F" | strings | head -n 8
  echo "ending sample:"
  tail -c 2048 "$F" | strings | tail -n 8
done

echo
echo "========== SMALL REPORTS WITH DUPLICATE CONTENT =========="
find "$ROOT" -maxdepth 1 -type f -size -10M -print0 |
  sort -z | xargs -0 -r sha256sum |
  sort | awk '{
    hash=$1
    $1=""
    sub(/^ /,"")
    if (hash==previous) {
      if (!shown) print previous_file
      print $0
      shown=1
    } else {
      previous_file=$0
      previous=hash
      shown=0
    }
  }'

echo
echo "========== REPORT DIRECTORY SUMMARY =========="
du -sh "$ROOT"
find "$ROOT" -maxdepth 1 -type f -size +1G -printf "%f  %s bytes\n" | sort

echo
echo "READ ONLY: no files moved, deleted, or modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
