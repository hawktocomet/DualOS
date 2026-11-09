# Inspect Duo EDL storage and cleanup candidates

Report ID: `20261109T031311Z-inspect-duo-edl-storage-and-cleanup-candidates`

Exit code: `0`

Command: `bash -lc 
ROOT="/home/hawk/duo_edl"

echo "========== FILESYSTEM / ROOT =========="
df -h "$ROOT"
du -sh "$ROOT"
du -sh --apparent-size "$ROOT"
echo

echo "========== TOP-LEVEL DIRECTORY SIZES =========="
du -x -h --max-depth=1 "$ROOT" 2>/dev/null | sort -h
echo

echo "========== SECOND-LEVEL DIRECTORY SIZES =========="
du -x -h --max-depth=2 "$ROOT" 2>/dev/null | sort -h | tail -n 100
echo

echo "========== LARGEST FILES: APPARENT VS ALLOCATED =========="
find "$ROOT" -xdev -type f -printf "%s\t%b\t%p\n" 2>/dev/null |
  sort -nr -k1,1 | head -n 100 |
  awk -F "\t" '{
    printf "apparent=%10.2f MiB allocated=%10.2f MiB  %s\n",
      $1/1048576, ($2*512)/1048576, $3
  }'
echo

echo "========== BUILD IMAGE INVENTORY =========="
find "$ROOT/build" -maxdepth 2 -type f \
  -printf "%TY-%Tm-%Td %TH:%TM  %s bytes  %p\n" 2>/dev/null | sort
echo

echo "========== REPORT FILE INVENTORY =========="
find "$ROOT/reports" -maxdepth 2 -type f \
  -printf "%TY-%Tm-%Td %TH:%TM  %s bytes  %p\n" 2>/dev/null | sort
echo

echo "========== TOP-LEVEL CONTENTS =========="
find "$ROOT" -mindepth 1 -maxdepth 1 \
  -printf "%y  %f\n" | sort
echo

echo "========== GIT STATUS =========="
for R in "$ROOT/edl" "$ROOT/FreeABL" "$ROOT/migration_work/duo-lite-reports"; do
  if [ -d "$R/.git" ]; then
    echo "--- $R"
    git -C "$R" status --short --branch 2>&1 | head -n 30
    git -C "$R" rev-parse --short HEAD 2>/dev/null
  fi
done
echo

echo "========== IMAGE FILE METADATA =========="
find "$ROOT" -xdev -type f \
  \( -name "*.img" -o -name "*.raw" -o -name "*.bin" -o -name "*.elf" \) \
  -printf "%s bytes  %p\n" 2>/dev/null | sort -nr | head -n 80
echo

echo "READ ONLY: no files moved, deleted, or modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
